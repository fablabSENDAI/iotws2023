---
layout: default
title: サンプルコード（音量センサ）
parent: セットアップ - 上級者向け
nav_order: 3
---

# サンプルコード（音量センサ）

M5StickCPlusに内蔵されているマイクを利用して周辺の環境音量を取得、Ambientへ送るサンプルプログラム。

**M5StickPlusC用プログラム**
```java
#include <M5StickCPlus.h>
#include <driver/i2s.h>

// AmbientとWIFIライブラリの読み込みとログイン情報を定義 ここから
#include <Ambient.h>
#include <WiFi.h>

const char* ssid = "?????????"; // ルータのアクセスポイント名
const char* password = "?????????"; // ルーターのパスワード

unsigned int channelId = ?????????; // AmbientのチャネルID
const char* writeKey = "?????????"; // Ambientのライトキー;

WiFiClient client;
Ambient ambient;

#define PIN_CLK 0
#define PIN_DATA 34
#define DATA_POINTS 512
#define SAMPLE_RATE 16000
#define READ_LEN (2 * DATA_POINTS)
#define GAIN_FACTOR 3
uint8_t BUFFER[READ_LEN] = { 0 };

int16_t *adcBuffer = NULL;

void i2sInit() {
  i2s_config_t i2s_config = {
    .mode = (i2s_mode_t)(I2S_MODE_MASTER | I2S_MODE_RX | I2S_MODE_PDM),
    .sample_rate = SAMPLE_RATE,
    .bits_per_sample = I2S_BITS_PER_SAMPLE_16BIT,  // is fixed at 12bit, stereo, MSB
    .channel_format = I2S_CHANNEL_FMT_ALL_RIGHT,
    .communication_format = I2S_COMM_FORMAT_I2S,
    .intr_alloc_flags = ESP_INTR_FLAG_LEVEL1,
    .dma_buf_count = 2,
    .dma_buf_len = 128,
  };

  i2s_pin_config_t pin_config;
  pin_config.bck_io_num = I2S_PIN_NO_CHANGE;
  pin_config.ws_io_num = PIN_CLK;
  pin_config.data_out_num = I2S_PIN_NO_CHANGE;
  pin_config.data_in_num = PIN_DATA;

  i2s_driver_install(I2S_NUM_0, &i2s_config, 0, NULL);
  i2s_set_pin(I2S_NUM_0, &pin_config);
  i2s_set_clk(I2S_NUM_0, SAMPLE_RATE, I2S_BITS_PER_SAMPLE_16BIT, I2S_CHANNEL_MONO);
}

void mic_record_task(void *arg) {
  size_t bytesread;
  while (1) {
    i2s_read(I2S_NUM_0, (char *)BUFFER, READ_LEN, &bytesread, (100 / portTICK_RATE_MS));
    adcBuffer = (int16_t *)BUFFER;
    processSignal();
    delay(1);  // もとのサンプルでは100msも待っているのを抜く
  }
}

void setup() {
  M5.begin();
  M5.Lcd.setRotation(3);
  M5.Lcd.setTextFont(4);
  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setTextColor(WHITE, BLACK);

  M5.lcd.println(F("MIC Volume Sender"));

  Serial.begin(115200);

  i2sInit();
  xTaskCreate(mic_record_task, "mic_record_task", 2048, NULL, 1, NULL);

  // チャネルIDとライトキーを指定してAmbientの初期化
  ambient.begin(channelId, writeKey, &client);
}

float filteredBase = 0;
float totalPower = 0;
int numPower = 0;

float calcDecibell(float x) {
  return 11.053 * log(x) + 11.142;
}

void processSignal() {
  // 平均を取ってゼロ点を自動的に補正する
  float base = 0;
  for (int n = 0; n < DATA_POINTS; n++) {
    base += adcBuffer[n];
  }
  base /= DATA_POINTS;

  // フィルタをかけて変化を緩やかにする
  const float alpha = 0.98;
  filteredBase = filteredBase * alpha + base * (1 - alpha);

  // Serial.printf("fb:%f,b:%f\n", filteredBase, base);

  // 二乗平均平方根を取る
  float power = 0;
  for (int n = 0; n < DATA_POINTS; n++) {
    power += sq(adcBuffer[n] - filteredBase);
  }
  power = sqrt(power / DATA_POINTS);

  Serial.printf("db:%f\n", calcDecibell(power));

  // あとで一定時間ごとに平均を取るため、グローバル変数に足し込んでおく
  totalPower += power;
  numPower++;
}

void loop() {

  int i = 0;
  int cnt = 0;
  float max_volume = 0.0;

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    cnt++;
    delay(500);
    M5.Lcd.print(".");
    if (cnt % 10 == 0) {
      WiFi.disconnect();
      WiFi.begin(ssid, password);
      M5.Lcd.println("");
    }
    if (cnt >= 30) {
      ESP.restart();
    }
  }

  while (i < 80) {
    if (numPower > 0) {
      // 蓄積されたデータの平均を取り、dBに変換する
      float p = totalPower / numPower;
      float db = calcDecibell(p);

      if (millis() > 20000) {
        if (db > max_volume) {
          max_volume = db;
        }
      }

      // 出力
      M5.lcd.fillRect(0, 20, 240, 115, BLACK);
      M5.lcd.setCursor(0, 20);
      M5.Lcd.printf("sound: %2.2f dB\r\n", db);

      // 次の計測に備えてリセット
      totalPower = 0;
      numPower = 0;
    }

    //カウントアップ
    i = i + 1;
    // 0.5秒待つ
    delay(500);
  }

  // Ambientに最大音圧を送信
  ambient.set(4, max_volume);
  ambient.send();
}
```

プログラム内のコメントにもある通り、[Ambientのデータ書き込み回数はMAX 3000件/日](https://ambidata.io/refs/spec/)です。なので、

> (24時間 * 60分 * 60秒) / 3000 =  28.8秒


最低でも↑の間隔でデータ送信しないとスグ制限に達してしまいます。プラス安全マージンとって40秒間隔でのデータ書き込み設定にしております。
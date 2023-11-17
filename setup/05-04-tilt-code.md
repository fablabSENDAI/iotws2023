---
layout: default
title: サンプルコード（傾きセンサ）
parent: セットアップ(上級者向け)
nav_order: 4
---

# サンプルコード（傾きセンサ）

M5StickCPlusに内蔵されている加速度センサを利用して、デバイス本体の傾きを取得、Ambientへ送るサンプルプログラム。

**M5StickPlusC用プログラム**
```java
#include <M5StickCPlus.h>

// AmbientとWIFIライブラリの読み込みとログイン情報を定義 ここから
#include <Ambient.h>
#include <WiFi.h>

const char* ssid = "?????????"; // ルータのアクセスポイント名
const char* password = "?????????"; // ルーターのパスワード

unsigned int channelId = ?????????; // AmbientのチャネルID
const char* writeKey = "?????????"; // Ambientのライトキー;

WiFiClient client;
Ambient ambient;

// --------------------------------------------------------
// 変数・定数定義部
// --------------------------------------------------------
float acc[3];  // 加速度測定値格納用（X、Y、Z）

float roll = 0.0F;  // ロール格納用

const float pi = 3.14;

// --------------------------------------------------------
// 関数定義部
// --------------------------------------------------------

// 加速度　取得用関数
void readGyro() {
  M5.IMU.getAccelData(&acc[0], &acc[1], &acc[2]);  // 加速度の取得

  roll = atan2(acc[0], acc[2]) * -180 / pi;  // roll角度計算
}

void setup() {
  // M5StickCの初期化と動作設定　Initialization and operation settings of M5StickC.
  M5.begin();  // 開始

  // IMUの初期化
  M5.Imu.Init();

  M5.Lcd.setRotation(3);
  M5.Lcd.setTextFont(4);
  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setTextColor(WHITE, BLACK);

  M5.lcd.println(F("Roll Angle Sender"));

  // チャネルIDとライトキーを指定してAmbientの初期化
  ambient.begin(channelId, writeKey, &client);
}

void loop() {

  int i = 0;
  int cnt = 0;

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
    //センサ値取得
    readGyro();

    //出力
    M5.lcd.fillRect(0, 20, 240, 115, BLACK);
    M5.lcd.setCursor(0, 20);
    M5.Lcd.printf("roll: %2.2f deg\r\n", roll);

    //カウントアップ
    i = i + 1;
    //0.5秒待つ
    delay(500);
  }

  //Ambientにroll角度を送信
  ambient.set(5, roll);
  ambient.send();
}
```

プログラム内のコメントにもある通り、[Ambientのデータ書き込み回数はMAX 3000件/日](https://ambidata.io/refs/spec/)です。なので、

> (24時間 * 60分 * 60秒) / 3000 =  28.8秒


最低でも↑の間隔でデータ送信しないとスグ制限に達してしまいます。プラス安全マージンとって40秒間隔でのデータ書き込み設定にしております。
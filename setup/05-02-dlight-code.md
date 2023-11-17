---
layout: default
title: サンプルコード（光センサ）
parent: セットアップ(上級者向け)
nav_order: 2
---

# サンプルコード（光センサ）

環境光センサ[DLight Unit](https://docs.m5stack.com/en/unit/DLight%20Unit)を使用したサンプルプログラム。

**M5StickPlusC用プログラム**
```java
#include <M5StickCPlus.h>
#include <M5_DLight.h>

// AmbientとWIFIライブラリの読み込みとログイン情報を定義 ここから
#include <Ambient.h>
#include <WiFi.h>

const char* ssid = "?????????"; // ルータのアクセスポイント名
const char* password = "?????????"; // ルーターのパスワード

unsigned int channelId = ?????????; // AmbientのチャネルID
const char* writeKey = "?????????"; // Ambientのライトキー;

WiFiClient client;
Ambient ambient;

M5_DLight sensor;
float lux;

void setup() {
  M5.begin();

  M5.Lcd.setRotation(3);
  M5.Lcd.setTextFont(4);
  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setTextColor(WHITE, BLACK);
  M5.lcd.println(F("Brightness Sender"));

  sensor.begin();

  sensor.setMode(CONTINUOUSLY_H_RESOLUTION_MODE);

  // チャネルIDとライトキーを指定してAmbientの初期化
  ambient.begin(channelId, writeKey, &client);
}


void loop() {
  int i = 0;
  int cnt = 0;

  //M5.Lcd.printf("Connecting to %s\n", ssid);
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

  while(i < 80){
  //センサ値取得
  lux = sensor.getLUX();

  //出力
  M5.lcd.fillRect(0, 20, 240, 115, BLACK);
  M5.lcd.setCursor(0, 20);
  M5.Lcd.printf("light: %2.0f\r\n", lux);

    //カウントアップ
    i = i + 1;
    //0.5秒待つ
    delay(500);
  }

  //Ambientにセンサ値を送信
  ambient.set(6, lux);
  ambient.send();
}
```

プログラム内のコメントにもある通り、[Ambientのデータ書き込み回数はMAX 3000件/日](https://ambidata.io/refs/spec/)です。なので、

> (24時間 * 60分 * 60秒) / 3000 =  28.8秒


最低でも↑の間隔でデータ送信しないとスグ制限に達してしまいます。プラス安全マージンとって40秒間隔でのデータ書き込み設定にしております。
---
layout: default
title: サンプルコード（環境センサ）
parent: セットアップ - 上級者向け
nav_order: 1
---

# サンプルコード（環境センサ）

環境センサ[ENV3](https://docs.m5stack.com/en/unit/envIII?id=description)を使用したサンプルプログラム。

基本的には、[ここのコード](https://blog.hrendoh.com/try-ambient-with-m5stickc-and-env-iii-unit/)をベースにしていますが、データを送る間隔と、データを送っていない間もセンサの値を更新し続ける、という改修を行なっています。


**M5StickPlusC用プログラム**
```java
#include <M5StickCPlus.h>
#include "Adafruit_Sensor.h"
#include <Adafruit_BMP280.h>
#include <M5_ENV.h>


// AmbientとWIFIライブラリの読み込みとログイン情報を定義 ここから
#include <Ambient.h>
#include <WiFi.h>

const char* ssid = "?????????"; // ルータのアクセスポイント名
const char* password = "?????????"; // ルーターのパスワード


unsigned int channelId = ?????????; // AmbientのチャネルID
const char* writeKey = "?????????"; // Ambientのライトキー;

WiFiClient client;
Ambient ambient;

SHT3X sht30;
QMP6988 qmp6988;

float tmp = 0.0;
float hum = 0.0;
float pressure = 0.0;

void setup() {
  M5.begin(); //Init M5StickC.
  
  M5.Lcd.setRotation(3);  //Rotate the screen.
  M5.Lcd.setTextFont(4);
  
  Wire.begin(); //Wire init, adding the I2C bus.
  qmp6988.init();
  M5.lcd.println(F("ENV Unit III Sender"));

  // チャネルIDとライトキーを指定してAmbientの初期化
  ambient.begin(channelId, writeKey, &client);  
  // WIFIとAmbientクライアントの初期化 ここまで
}

void loop() {

  int i = 0;
  int cnt = 0;
  
  WiFi.begin(ssid, password);
  while(WiFi.status() != WL_CONNECTED) {
    cnt++;
    delay(500);
    M5.Lcd.print(".");
    if(cnt%10==0) {
      WiFi.disconnect();
      WiFi.begin(ssid, password);
      M5.Lcd.println("");
    }
    if(cnt>=30) {
      ESP.restart();
    }
  }

  while(i<24){
   pressure = qmp6988.calcPressure() / 100;
   if(sht30.get()==0){ //Obtain the data of shT30.
    tmp = sht30.cTemp;  //Store the temperature obtained from shT30.
    hum = sht30.humidity; //Store the humidity obtained from the SHT30.
   }else{
    tmp=0,hum=0;
   }
   M5.lcd.fillRect(0, 20, 240, 115, BLACK); //Fill the screen with black (to clear the screen).
   M5.lcd.setCursor(0,20);
   M5.Lcd.printf("Temp: %2.1f  \r\nHumi: %2.0f%%  \r\nPressure:%2.1f hPa\r\n", tmp, hum, pressure);
  
  //カウントアップ
   i = i + 1;

   //5秒おきに測定値を画面表示
  delay(5 * 1000); 
  }

  // Ambientに気温、湿度、気圧を送信 ここから
  ambient.set(1, tmp);
  ambient.set(2, hum);
  ambient.set(3, pressure);
  ambient.send();
  // Ambientに気温、湿度、気圧を送信 ここまで

  // Ambientの3000件/日の制限を超えないように計120秒待機
  delay(5 * 1000);
```

プログラム内のコメントにもある通り、[Ambientのデータ書き込み回数はMAX 3000件/日](https://ambidata.io/refs/spec/)です。なので、

> (24時間 * 60分 * 60秒) / 3000 =  28.8秒


最低でも↑の間隔でデータ送信しないとスグ制限に達してしまいます。<br>しかも今回は、３つ（温度、湿度、気圧）のデータを送るので、これを三倍して、


> 28.8秒 * 3 = 86.4秒


という間隔でデータ送信しないといけません。プラス安全マージンとって120秒間隔でのデータ書き込み設定にしております。
---
layout: default
title: セットアップ(上級者向け)
nav_order: 6
---

# セットアップ(上級者向け)
**※この章は上級者向けの補足資料です！**

今回配った、デバイスをより深く知りたい。セミナー終了後も使い続けたいという方に向けて、どうやって講師側がセットアップしたのかをココに記録しておきます。大まかな手順としては...

1. wifiルーター & IoT向けSimカードの入手
2. Ambientのアカウント作成
3. M5StickPlus & 環境センサ―の入手 & プログラミング

と、なります。

---

<br><br>

## STEP1 : モバイルルーター & IoT向けSimカードの入手
まずは、このツールキット専用のインターネット環境を作るために、モバイルルーターとSIMカードを手配します。

モバイルルーターは、[格安通販サイト](https://iosys.co.jp/)で中古のものを探しても良いでしょう。<br>今回は、セキュリティの面からNEC製のドコモ回線モバイルルーター["Wi-Fi STATION N-01J"](https://www.docomo.ne.jp/support/product/n01j/)を調達しました。


SIMカードは、IoTデバイス向けのSIMを販売している[SORACOM](https://soracom.jp/)のものを使っています。モバイルルーターがドコモ回線機なので、[それに合わせたプラン](https://soracom.jp/services/air/cellular/pricing/price_specific_area_sim/)を選択しました。詳細は下記にて↓

> SORACOM Air for セルラー, 特定地域向けIoT SIM, plan-D D-300MB, 速度クラス : s1.standard

<br><br>

SIMカードとモバイルルーターが手に入ったら、SIMカードをルーターにセットし、ルータのAPN設定を[公式ページのガイド](https://users.soracom.io/ja-jp/guides/getting-started/setup/)通りに行なえば、通信が開始されます。


通信開始後、ルーターの **アクセスポイント名** と **パスワード** をメモしておきましょう。後で使います。

---

<br><br>

## STEP2 : Ambientのアカウント作成
ここからのSTEP2とSTEP3については、[このチュートリアルページ](https://blog.hrendoh.com/try-ambient-with-m5stickc-and-env-iii-unit/)を参考にしていますので、そちらも合わせて読んでいただくと分かりやすいと思います。


まずは、[Ambientのウェブページ](https://ambidata.io/)を開き、アカウントを作成します。<br>自分のアカウントにログインしたら、"チャネル一覧"を開き、<br>この画面で左下に表示されている"チャネルを作る"をクリックします。

<img src="images\ambient_003.jpg" alt="hi" class="inline" border="1"/>

デフォルトではチャネルリストには何も表示されていませんが、ココで初めてチャネルが作成され上記の画像のように表示されます。

チャネルが作成されたら、"・・・"と表示されている設定メニューをクリックし、"設定変更"をクリックすると、チャネルの設定画面に移るので、チャネルの名前や受け取るデータの名前を編集します。今回のIoTツールの場合は下の画像のとおり設定しました。

<img src="images\ambient_019.jpg" alt="hi" class="inline" border="1"/>


変更したのは、チャネル名、説明、データ-1~3の名前です。<br>編集が終了したら画面下にある"チャネル属性を設定する"というボタンをクリックして設定を保存します。


チャネル一覧に戻ってきたら、今作ったチャネルの **"チャネルID"** と **"ライトキー"** をメモしておきましょう。後で使います。


以上でAmbient側の準備も完了です。

---

<br><br>


## M5StickCPlusのプログラミング

[M5StcikCPlus](https://www.switch-science.com/catalog/6470/)、および[環境センサー](https://www.switch-science.com/catalog/7254/)はどちらも[SwitchScience](https://www.switch-science.com/)から購入しました。

M5StickCPlusは、ArduinoIDEからプログラムを書き込むことが出来ます。(チュートリアルは[コチラ](https://docs.m5stack.com/en/quick_start/m5stickc_plus/arduino))

基本的には、[ここのコード](https://blog.hrendoh.com/try-ambient-with-m5stickc-and-env-iii-unit/)をベースにしていますが、データを送る間隔と、データを送っていない間もセンサの値を更新し続ける、という改修を行なっています。


**M5StickPlusC用プログラム**
```java
#include <M5StickCPlus.h>
#include "Adafruit_Sensor.h"
#include <Adafruit_BMP280.h>
#include <M5_ENV.h>
//#include "UNIT_ENV.h"


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
  M5.lcd.println(F("ENV Unit III test"));

  // チャネルIDとライトキーを指定してAmbientの初期化
  ambient.begin(channelId, writeKey, &client);  
  // WIFIとAmbientクライアントの初期化 ここまで
}

void loop() {

  int i = 0;
  int cnt=0;
  
  //M5.Lcd.printf("Connecting to %s\n", ssid);
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
   Serial.println(pressure);
   M5.lcd.fillRect(0,20,100,60,BLACK); //Fill the screen with black (to clear the screen).
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
}
```

プログラム内のコメントにもある通り、[Ambientのデータ書き込み回数はMAX 3000件/日](https://ambidata.io/refs/spec/)です。なので、

> (24時間 * 60分 * 60秒) / 3000 =  28.8秒


最低でも↑の間隔でデータ送信しないとスグ制限に達してしまいます。<br>しかも今回は、３つ（温度、湿度、気圧）のデータを送るので、これを三倍して、


> 28.8秒 * 3 = 86.4秒


という間隔でデータ送信しないといけません。プラス安全マージンとって120秒間隔でのデータ書き込み設定にしております。


<br><br>

---


<br><br>


M5Stickに上記プログラムが書き込めれば、あとはセミナーの本内容と同じことが出来ます。今回は使用したSIMとモバイルルーターは返却いただきますが、自前でSORACOMとルーターの手配が出来れば、再度環境構築することも可能です。
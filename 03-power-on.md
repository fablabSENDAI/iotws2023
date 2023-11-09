---
layout: default
title: IoTツールの使い方
nav_order: 4
---

# IoTツールの使い方
本セミナーで使用するIoTツールの接続方法と、電源の入れ方を説明します。

特に、電源を入れる順番を間違えるとツールがインターネットに繋がりにくい場合がありますので、注意してください。

<br><br>

---
## STEP0 : IoTツールの内容
今回こちらで用意したIoTツールは以下の４つです。<br><br>
<img src="images\tools_001_allKit.jpg" alt="hi" class="inline"/>

- **モバイルルーター**
   - IoT専用SIMカードを搭載したモバイルルーター。IoT専用SIMはデータ容量がとても少ない(300MB / 月)ので、**スマホやPCでの接続は絶対にやめてください。** 容量がオーバーしたルーターは自動で回線がストップします。
- **M5StickC Plus**
   - このIoTツールの頭脳ともいうべきパーツです、小型バッテリー内臓のマイコンで書き込まれたプログラムを実行してくれます。今回は、センサから読み取った各値をルーター経由でインターネットへ転送してくれます。
- **環境センサー**
   - M5StickC Plusに接続する環境センサ―です。これ１つで周囲の気温、湿度、気圧を測ることが出来ます。
- **分岐USB-Cケーブル**
   - モバイルルーター、M5StickCPlusはどちらも、USB-Cアダプタで充電を行ないます。このため、USB-Cケーブルを２本用意する必要があります。<br>そこで、普通のUSBアダプタ(Aタイプ)から給電できるよう、USB-Aアダプタ(1口) → USB-Cアダプタ(２口)で接続できるUSBケーブルを同梱しました。


<br><br>


ツールはUSB電源が一口あれば動きます。なので、PCのUSB-Aアダプタに繋いで動かしても良いのです。しかし、24時間1~2週間センサのログを取りたい時などPCも電源入れっぱなしなのは不安なので、以下のようなUSB電源アダプタがあると、なお良いでしょう↓

<img src="images\tools_002_acAdpt.jpeg" alt="hi" class="inline"/>

*モバイルバッテリーも容量によって持ちは変わりますが、ぜひチャレンジしてみてくださいー！*
<br><br>

---

## STEP1 : モバイルルーターの電源ON
まずは、モバイルルーターの電源から入れていきます。電源ボタンはディスプレイ正面からみて右上の角にあります。

<img src="images\tools_003_powerSwitch.jpg" alt="hi" class="inline"/>

先程のUSBケーブルで電源とルータを繋いだら、<br>電源ボタンを長押しして、ディスプレイに立ち上げアイコンが表示されるのを待ちましょう。

<img src="images\tools_004_powerRouter.jpeg" alt="hi" class="inline"/>

↑この状態になったらしばらく、そのままにしておきます。

しばらく経つと、ロック画面が表示されるので、錠前マークを長押ししてロックを解除します。

<img src="images\tools_006_unlockDisp.jpg" alt="hi" class="inline"/>

！！！注意！！！<br>
ロックを解除すると、以下のようなメッセージが表示されることがあります。<br>
この場合は、**必ず"いいえ"をタッチ**してください。

もし"はい"をタッチしてしまうと、ルーターのアップデートが始まり大量のデータを通信してしまいます。<br>その結果、SIMの通信が停止してしまう可能性があるので気を付けて下さい。

<img src="images\tools_005_routerUpdateNo.jpg" alt="hi" class="inline"/>

起動が完了し、ロックを解除すると以下の画面が表示されます。

<img src="images\tools_007_homeDisp.jpeg" alt="hi" class="inline"/>

これで、モバイルルーターの電源が入りました。<br>電源が入ったあとで、何もせずしばらく経つと画面が暗くなる事がありますが、再度電源ボタンを押すと画面が立ち上がります。


※ もし、電源を切りたい場合は電源を切りたい場合は、電源ボタンを長押しすると電源メニューが表示されるので、その中から"電源OFF"をタッチすると、電源OFFになります。
<br><br>

---

## STEP2 : 環境センサをつなぐ
環境センサ―をM5StickC PLUS(以下、M5Stick)に繋げておきます。
環境センサーとM5Stickを付属のケーブルで下の写真のように接続してください。

<img src="images\tools_010_stickCconnect.jpg" alt="hi" class="inline"/>

<br><br>

---

##  STEP3 : M5StickC PLUSの電源ON
モバイルルーターの電源が入り、環境センサ―を接続したら、M5Stickの電源も入れましょう。
まずは、USBケーブルをM5Stickにも繋げます。先程、センサーを繋げたアダプタに隣接しています。給電が始まると、M5Stickは自動的に電源ONになります。

<img src="images\tools_012_stickCPower.jpg" alt="hi" class="inline"/>
もし、電源が入らなかった場合は、以下の電源ボタンを押して電源を入れてください。

<br>

M5Stickの電源ボタンは"M5"の文字を正面にして、左側面にあります。<br>逆側にも似たようなボタンがあるので注意してください。

このボタンを、 **１回押すと電源ON。６秒以上長押しすると電源OFF。** となります。

<img src="images\tools_008_stickC.jpg" alt="hi" class="inline"/>

もし、不具合があった場合は、<br>M5Stickの電源を切って、再度立ち上げると解決することがあるので、電源の入れ方、切り方は覚えておいてください。


電源が入り、ルーターと正常に通信を開始すると、<br>M5Stickの画面には、以下のようにセンサの各測定値が表示されます。

- Temp : 気温
- Humi : 湿度
- Pressure : 気圧

<img src="images\tools_014_sensorState.jpeg" alt="hi" class="inline"/>

この状態になると、大体２分おきにインターネットへ計測したデータをアップロードし始めます。<br>もし、長時間データのアップを続けたいのであれば、USBケーブルと接続して給電しながら稼働させましょう。

次の章では、インターネットへアップロードした測定値の見方を解説します。
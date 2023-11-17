---
layout: default
title: 光センサの準備方法
parent: IoTツールの使い方
nav_order: 2
---

# 光センサの準備方法

## センサと接続する

光センサ―をM5StickC PLUS(以下、M5Stick)に繋げておきます。
光センサーとM5Stickを付属のケーブルで下の写真のように接続してください。

<img src="../images/tools_021_stickConnectDlight.jpg" alt="hi" class="inline"/>

<br><br>

---

## M5StickC PLUSの電源ON
モバイルルーターの電源が入り、光センサ―を接続したら、M5Stickの電源も入れましょう。
まずは、USBケーブルをM5Stickにも繋げます。先程、センサーを繋げたアダプタに隣接しています。給電が始まると、M5Stickは自動的に電源ONになります。

<img src="../images/tools_012_stickCPower.jpg" alt="hi" class="inline"/>
もし、電源が入らなかった場合は、以下の電源ボタンを押して電源を入れてください。

<br>

M5Stickの電源ボタンは"M5"の文字を正面にして、左側面にあります。<br>逆側にも似たようなボタンがあるので注意してください。

このボタンを、 **１回押すと電源ON。６秒以上長押しすると電源OFF。** となります。

<img src="../images/tools_008_stickC.jpg" alt="hi" class="inline"/>

もし、不具合があった場合は、<br>M5Stickの電源を切って、再度立ち上げると解決することがあるので、電源の入れ方、切り方は覚えておいてください。


電源が入り、ルーターと正常に通信を開始すると、<br>M5Stickの画面には、以下のようにセンサの各測定値が表示されます。

- light : 明るさ (数値が大きいほど明るい)

<img src="../images/tools_016_dlightStatus.jpg" alt="hi" class="inline"/>

この状態になると、大体２分おきにインターネットへ計測したデータをアップロードし始めます。<br>もし、長時間データのアップを続けたいのであれば、USBケーブルと接続して給電しながら稼働させましょう。
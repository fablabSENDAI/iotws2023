---
layout: default
title: セットアップ - 上級者向け
nav_order: 6
has_children: true
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

モバイルルーターは、[格安通販サイト](https://iosys.co.jp/)で中古のものを探しても良いでしょう。<br>今回は、セキュリティの面からNEC製のドコモ回線モバイルルーター["Wi-Fi STATION N-01J"](https://www.docomo.ne.jp/support/product/n01j/)を調達しました。今回使用するSIMカードがSoftbank回線を利用するので、SIMロックも解除してあります。


SIMカードは、IoTデバイス向けのSIMを販売している[1NCE](https://1nce.com/ja-jp/)のものを使っています。

<br><br>

SIMカードとモバイルルーターが手に入ったら、SIMカードをルーターにセットし、ルータのAPN設定を[公式ページのガイド](https://1nce.com/ja-jp/support/faq/where-can-i-find-the-apn)通りに行なえば、通信が開始されます。


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

使用するセンサに対応したプログラムをM5StickCPlusに書き込みます。

M5StickCPlusは、ArduinoIDEからプログラムを書き込むことが出来ます。(チュートリアルは[コチラ](https://docs.m5stack.com/en/quick_start/m5stickc_plus/arduino))

各センサのプログラムについては、以下のリンクを参照してください。

- [環境センサ](https://fablabsendai.github.io/iotws2023/setup/05-01-env3-code.html)
- [光センサ](https://fablabsendai.github.io/iotws2023/setup/05-02-dlight-code.html)
- [音量センサ](https://fablabsendai.github.io/iotws2023/setup/05-03-mic-code.html)
- [傾きセンサ](https://fablabsendai.github.io/iotws2023/setup/05-04-tilt-code.html)

なお、[M5StcikCPlus](https://www.switch-science.com/catalog/6470/)、および各種センサーは[SwitchScience](https://www.switch-science.com/)から購入しました。





---


<br><br>


M5Stickに上記プログラムが書き込めれば、あとはセミナーの本内容と同じことが出来ます。今回は使用したSIMとモバイルルーターは返却いただきますが、自前で1NCEとルーターの手配が出来れば、再度環境構築することも可能です。
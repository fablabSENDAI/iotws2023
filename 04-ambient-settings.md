---
layout: default
title: Ambientの設定方法
nav_order: 5
has_children: true
---

# Ambientの設定方法
M5Stickからインターネットに送られたデータは、Ambient(アンビエント)というIoT向けデータ表示サイトへ送られています。

**[→ Ambientへ移動](https://ambidata.io/)**

このサイトでは、IoTツールから送られてきたデータの変化を折れ線グラフで見ることが出来ます。

各自のIoTツールと共に同梱されている、Ambient用のメールアドレスとパスワードが書かれたメモを手元に用意して、基本的な使い方を試してみましょう。

<br>

---

<br><br>

## STEP1 : Ambientにログイン

まずは、ブラウザで[Ambient](https://ambidata.io/)にアクセスします。<br>TOP画面右上の **"ログイン"** ボタンをクリック。

<img src="images\ambient_001.jpg" alt="hi" class="inline" border="1"/>

すると、ログイン画面に移リます。<br>各自に配られたメモに書かれたメールアドレスとパスワードをそれぞれの欄に入力して下さい。<br>入力し終えたら、**ログイン**ボタンをクリック。

<img src="images\ambient_002.jpg" alt="hi" class="inline" border="1"/>


ログインが完了すると、以下の画面が表示されます↓


<img src="images\ambient_003.jpg" alt="hi" class="inline" border="1"/>

<br>

---

<br><br>

## STEP2 : 見たいチャネルを選択

これは、現在自分のアカウントに対して送られているチャネル(データの入り口)のリストです。

<img src="images\ambient_003.jpg" alt="hi" class="inline" border="1"/>

ここで、Ambient内で使われている２つの用語を簡単に説明しておきます。


- **チャネル**
  - アカウントに送られてくるデータの入り口のこと。１つのデバイスが複数のチャネルにデータを送信することもできるし、反対に１つのアカウントが複数のチャネルを持つこともできる。

- **ボード**
  - チャネルに送られてきたデータを表示させるページの事。本セミナーでは、１つのボードに１つのチャネルのデータを表示する。設定によっては、１つのボードに複数のチャネルのデータをまとめて表示することもできる。

まずは、画面左上のメニューからボード一覧をクリックして、チャネルが受け取ったデータを表示してくれるボードのリストを見に行きます。

<img src="images\ambient_008.jpg" alt="hi" class="inline"/>

<br>

そうするとボードのリストが表示されるのですが、現在はデフォルトで用意した１つのみなので、これをクリックします。

<img src="images\ambient_020.jpg" alt="hi" class="inline"/>

<br>

<img src="images\ambient_004.jpg" alt="hi" class="inline"/>

<br>

環境センサの場合、チャネルに送られている３つのデータ(温度、湿度、気圧)の折れ線グラフが表示されます。<br>
明るさ、音量、傾きセンサの場合、ボードには１つのグラフが表示されます。

小項目では更に細かいAmbientの設定を説明します。
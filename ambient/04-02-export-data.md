---
layout: default
title: データのダウンロード
parent: Ambientの設定方法
nav_order: 2
---

# データのダウンロード
Ambientから記録しているデータをCSV形式でダウンロードすることが出来ます。<br>CSVでダウンロードすればExcelでデータを編集できるようになるので、データ分析はもちろん、資料への転用もやりやすくなります。

## CSVデータのダウンロード
まずは、ボードの名前を変えた時と同じくボード名をクリックしてメニューを表示させます。

ただ、ここでボード一覧ではなく、**"チャネル一覧"** をクリックしてください。

<img src="..\images\ambient_012.jpg" alt="hi" class="inline" border="1"/>

そうすると、自分のアカウントが受け取っているチャネル(データの入り口)のリストが表示されます。

(現状では、チャネルが１つしか表示されないはずです。)


そのチャネルの右側にあるダウンロードボタンをクリックしてください。

<img src="..\images\ambient_017.jpg" alt="hi" class="inline" border="1"/>


すると、別ウィンドウが表示されるので、ダウンロードしたい日時を入力すると、<br>CSVデータがダウンロードされます。


## CSVデータをExcelで見てみる
ダウンロードしたデータをExcelで開いてみると、下のように表示されます。


A列がデータが送信された日時なのですが、表示がおかしくなっているので、設定します。

<img src="..\images\ambient_013.jpg" alt="hi" class="inline" border="1"/>


A列を選択して右クリック、メニューから **"セルの書式設定"** をクリックします。

<img src="..\images\ambient_014.jpg" alt="hi" class="inline" border="1"/>

書式設定のウィンドウで、表示形式タブに行き、


分類：日付の中から、時間付きの日付を選択して、


OKボタンを押して、設定を保存します。

<img src="..\images\ambient_018.jpg" alt="hi" class="inline" border="1"/>


すると、先のデータが以下のように表示されます。


<img src="..\images\ambient_015.jpg" alt="hi" class="inline" border="1"/>


日時も正しく表示されるようになって、見やすくなりました。


あとは、Ｅxcelデータとして別名保存し、従来のシート同様に編集が行うことが出来ます。
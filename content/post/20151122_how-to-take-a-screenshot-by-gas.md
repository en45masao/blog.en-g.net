+++
date = "2015-11-22"
draft = false
title = "Google Apps Scriptを使ってWebサイトのスクリーンショットを撮影する"
tags = ["Google Apps Script"]

+++

一年ほど前から「[Google Apps Script](https://developers.google.com/apps-script/) (以下、GAS)」を使い始めていて、定期スクレイピング・Twitter Bot・簡易Web APIなど、いろんなことに使い倒している。このGASを使って無理やりWebサイトのスクリーンショットを取得する方法を発見したのでメモしておく。

以下は[The Acid3 Test](http://acid3.acidtests.org)のページのスクリーンショットを保存する例。

	function test() {
	  var data = Charts.newDataTable()
	  .addColumn(Charts.ColumnType.STRING, 'dummy')
	  .addRow(['<meta http-equiv="refresh" content="0; URL=http://acid3.acidtests.org">'])
	  .build();

	  var chart = Charts.newTableChart()
	  .setDataTable(data)
	  .setOption('allowHtml', true)
	  .setDimensions(720, 480)
	  .build();

	  var blob = chart.getAs('image/png').setName('acid3.png');

	  DriveApp.createFile(blob);
	}

Google Appsのスクリプトエディタで上記のコードを入れて実行すると、Google Drive内に以下のような画像が保存される。[^1]

![Screen shot of The Acid3 Test by GAS](/post/20151122_acid3.png)

やっていることはけっこう無茶。Google Docsに埋め込み可能なグラフの中には「[Table Chart](https://developers.google.com/apps-script/reference/charts/table-chart-builder)」という表形式のチャートがあって、この表の個々のセルに入れる文字列データはHTMLタグを使って装飾することができる。で、上のサンプルコードでは「1×1のテーブルを作って、そのセルに`<meta http-equiv="refresh">`タグを入れて、スクリーンショットを取りたいURLにリダイレクトする」という強引なことをやっている。サーバー内部ではHTMLタグのレンダリングにChromeなりBlinkなりが動いているらしく、これで作った「グラフ」はリダイレクト先のページがそのままレンダリングされる。このグラフを画像blobに変換して、Googleドライブに保存している。

当然、こんなハック臭のする手法が将来的に安定して動作するとは思えないものの、そもそもGASのAPI自体、旧式のものはどんどんdeprecatedになっていたりするわけで、動いているうちは活用するという態度でもいいような気はする。

[^1]: 結果は「100/100」ではあるものの、「YOU SHOULD NOT SEE THIS AT ALL」の文字が見えているなど、レンダリングがおかしい部分もある。

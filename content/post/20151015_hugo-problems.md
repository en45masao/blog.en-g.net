+++
date = "2015-10-15"
draft = false
title = "Hugoを使っていて困っている点"
tags = ["Hugo"]

+++

[Hugo](http://gohugo.io/)を使ったブログが一応形になってきたものの、やりたいのに実現できていないことがいろいろあるので、備忘録も兼ねて、Hugoを使っていて困っている点を挙げてみる。

* 年別・月別の記事一覧を出せるようにしたい
	* 一般的なブログにはたいていあるやつ。
	* さらに言うと、ここのサイトでは各記事のURLを`//example.com/2015/10/15/`みたいにしているけど、これを`//example.com/2015/10/`としたら「2015年10月分」が、`//example.com/2015/`としたら「2015年分すべて」が表示されるようにしたい。
* 記事ごとに言語(lang属性)を指定したい
	* 今のところ日本語の記事しか書いてないけど、英語の記事を書くときは(適当な要素に)`lang="en"`を指定して、CSSを使って英語部分には別のスタイルを指定できるようにしたい。
	* テンプレート変数に`.Site.LanguageCode`というのがあるけど、これってたぶん「サイト全体」の言語指定だよね……。
* ある記事でしか使わない画像ファイルの置き場所をどうするか
	* 例えば、ある記事の中で説明用にスクリーンショットの画像を作ったとして、仮にその記事のURLが`//example.com/2015/10/15/`だとしたら、画像ファイルも`//example.com/2015/10/15/screenshot.png`に配置したい
	* もちろん、staticディレクトリ内に`static/2015/10/15/screenshot.png`みたいにディレクトリを掘って格納するのがstraightforwardな解ではあろうけど、いちいちそれやるのもめんどうだし、運用上、記事に付随する画像ファイルとかはcontentディレクトリ内に閉じる形で管理したいという思いもある。
* Markdownで定義リストが使えるようにしたい
	* これは[以前の記事](/2015/10/13)でも書いたとおり、最新版のソースでは使えるらしいので、次バージョンのリリース待ち。
	* これを機にGo言語の環境を整えてみるのもいいかもしれない。
* Markdownで出力した見出しの`id`属性の末尾にmd5文字列がつくのを抑止したい
	* URLの一部分にもなる文字列なので、なるべくシンプルにしたい。
	* `id`属性に見出しのテキストが(日本語のまま)追加されるのは、config.tomlの`[blackfriday]`セクションで`extensionsmask = ["autoHeaderIds"]`を追加することで抑止した。
* Markdownで出力したtableに`class="table"`を付けたい
	* これを付けないと、Bootstrapではテーブルをきれいに表示してくれない。
	* この件に限らず、MarkdownのHTMLレンダリングをカスタマイズする汎用的な手段があるのかどうか。


余談
--------------------------------

「ブログをHugoに移行した」という話はよく聞くし、ブログ用に作られたHugoのテーマもいろいろ見つかるものの、いわゆるふつーのブログにあるような機能を網羅しているような事例はあまり見つからなかったりする。Hugoベースのブログって、なんというか「シンプル」なブログが多いんですよね。

なので、やりたいことを実現する方法はないものかとHugoの公式サイトのドキュメントを眺めているわけですが、Hugoのテンプレート周りについてはいまいち理解が追いつかなくて苦労しているところ。Hugoのドキュメントを見るだけではなくて、前提知識としてGo言語標準のテンプレートエンジンの仕様も見ないと分からない、みたいな話なのかもしれず。
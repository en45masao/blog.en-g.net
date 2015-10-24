+++
date = "2015-10-13"
draft = false
title = "Hugoにおける拡張Markdown記法の定義リストのサポート状況"
tags = ["Hugo"]

+++

以前[Hugoで最初に書いた記事](/2015/09/27/)にて、Markdown拡張の定義リスト(`<dl>`・`<dt>`・`<dd>`)が使えないので後で調べないと、みたいなことを書いてからだいぶ時間がたったけど、ようやく重い腰を上げてHugoのMarkdownレンダリングまわりについて調査してみた。

Hugoで使っているMarkdownパーザーは[Blackfriday](https://github.com/russross/blackfriday)とのことで、READMEを見る限りでは、世の中のメジャーなMarkdown拡張記法を含むいろいろな機能をサポートしているように見える。

で、Hugoの公式ドキュメントを眺めていると「[Configure Blackfriday rendering](https://gohugo.io/overview/configuration/#configure-blackfriday-rendering:a66b35d20295cb764719ac8bd35837ec)」というセクションがあり、このへんのオプションをあれこれすることでBlackfriday側の設定を変えることができるらしい。ざっと見たところ、導入したい拡張機能の名前を`extensions`に列挙すればよさそうだが、そのへんはBlackfridayのソースを見て察しろという感じの投げっぷり。実際に`extensions`に指定する具体的な名前は、[Blackfridayのmarkdown.goで定義されている定数名](https://github.com/russross/blackfriday/blob/v1.3/markdown.go#L31)から頭の`EXTENSION_`を削ってlower camel caseにすればいいみたい。

[Blackfriday](https://github.com/russross/blackfriday)の[ソース](https://github.com/russross/blackfriday/blob/v1.3/markdown.go#L47)を見るに`EXTENSION_DEFINITION_LISTS`というのがあるので、以下のようにconfig.tomlの`extensions`に`"definitionLists"`を追加してみたが、定義リストは使えなかった。

	[blackfriday]
	  extensions = ["definitionLists"]

ちょっと調べたところ、最新のソースでは、Hugo側での`definitionLists`の指定が、Blackfriday側の`EXTENSION_DEFINITION_LISTS`にちゃんとマッピングされていた。手元で使っているv0.14の時点では、この修正がまだ入っていない模様。

* https://github.com/spf13/hugo/blob/v0.14/helpers/content.go#L59
* https://github.com/spf13/hugo/commit/82cc1ac0f8860de6ca097235d23babb42c69c720

というわけで、定義リストを使いたい人は、最新版のソースを取ってきてビルドするか、あるいは次のバージョンのバイナリがリリースされるまで待て、ということですかね。

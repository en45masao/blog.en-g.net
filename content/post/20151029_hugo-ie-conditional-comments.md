+++
date = "2015-10-29"
draft = false
title = "HugoでIEの条件付きコメントを使う"
tags = ["Hugo", "webdev"]

+++

Internet Explorerでこのブログの表示を確認していた際、IE8互換モードに切り替えると、なぜか指定したはずのスタイルがいくつか無効になっている。あれーもしかしてこれはIE8でHTML5のタグを認識しないことに起因する症状かな、でも[html5shiv](https://github.com/afarkas/html5shiv)を入れているからその点だいじょうぶなはずなんだけど……などと思いながらHTMLソースを見ると、html5shivを読み込んでいる`<script>`タグの行がごっそり消えていた。

ちょっと調べてあっさり解決。Hugoのテンプレートエンジン(というかGo言語のテンプレートライブラリ)は、デフォルトではHTMLコメントを削除した形でHTMLを出力するとのこと。これが原因で、ヘッダー部分のテンプレートに埋め込んでいたIEの条件付きコメント(`<!--[if lt IE 9]>`～`<![endif]-->`)が消えていたのでした。以下の公式ドキュメントの記載通りに記載して、無事修正。

> [Internet Explorer conditional comments using Pipes](https://gohugo.io/templates/go-templates/#internet-explorer-conditional-comments-using-pipes:e2fc23c6497b774f0cb0b339042e10c3)
> --------------------------------
> 
> By default, Go Templates remove HTML comments from output. This has the unfortunate side effect of removing Internet Explorer conditional comments. As a workaround, use something like this:
> 
	{{ "<!--[if lt IE 9]>" | safeHTML }}
	  <script src="html5shiv.js"></script>
	{{ "<![endif]-->" | safeHTML }}

- - -

そもそも今どきIE8向けの対応なんてしなくてもいいのでは、という気持ちもあるけれど、ブログのような文字ベースのコンテンツであれば、レガシーなブラウザであっても読むだけならできるので、ツールやライブラリの選択もやや保守的な方向に倒している(Bootstrap 3とかjQuery ver.1系とか)。さすがにWebアプリ的なコンテンツを作るとなると、古いブラウザは考慮しませんけどね……。

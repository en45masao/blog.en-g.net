+++
date = "2015-11-03T21:28:23+09:00"
draft = true
title = "URLにパラメータを指定するとアイコンフォントの画像をSVGで返すWebサービスがあると便利かも"
tags = ["webdev"]

+++

<svg width="640" height="400" viewBox="-272 -144 640 400">
	<defs>
		<style type="text/css">
		svg {
			fill: white;
			background-color: #287d1a;
		}
		</style>
	</defs>
	<g transform="scale(0.0625, 0.0625)">
		<svg width="1536" height="1792">
			<g transform="translate(0, 1536) scale(1.0, -1.0)">
				<path d="M1468 1156q28 -28 48 -76t20 -88v-1152q0 -40 -28 -68t-68 -28h-1344q-40 0 -68 28t-28 68v1600q0 40 28 68t68 28h896q40 0 88 -20t76 -48zM1024 1400v-376h376q-10 29 -22 41l-313 313q-12 12 -41 22zM1408 -128v1024h-416q-40 0 -68 28t-28 68v416h-768v-1536h1280z M1280 320v-320h-1024v192l192 192l128 -128l384 384zM448 512q-80 0 -136 56t-56 136t56 136t136 56t136 -56t56 -136t-56 -136t-136 -56z" />
			</g>
		</svg>
	</g>
</svg>

昨今のブログでは、記事の冒頭にいわゆる「アイキャッチ画像」を置いていることが多い。そういうの、自分でもやってみたいなー、と思わなくはないものの、素材を用意したり管理したりするのが面倒そうなのと、画像のせいで無駄に転送量が多くなるのは(特にモバイル向けで)いかがなものか、という思いがあり、さしあたりこのブログは文字ばかりという状態。

そういったことを考えている際にふと思いついたのが、タイトルにある「URLにパラメータを指定すると、([Font Awesome](https://fortawesome.github.io/Font-Awesome/)等の)アイコンフォントの画像をSVG形式で返すサービス」というもの。例えば、以下のようにパラメータ付きのURLを`<img>`タグの`src`属性に指定するだけで上にあるようなSVG画像が表示されるサービスがあれば、HTML一行だけでシンプルなアイキャッチ画像が埋め込めて便利そうな気がする。[^1]

`<img src="http://iconfont2img.example.com/?name=fa-file-image-o&size=640x400&region=96x96&background=#287d1a">`

これくらいなら、実装だけなら簡単にできそう。今どきは、Font Awesomeのような高品質でオープンソースライセンスのアイコンフォントがいろいろあるし。とりあえずメモとして、頭の中に思いついた仕様をメモしておく。なお、パラメータの体系は[ImageMagick](http://www.imagemagick.org/script/command-line-options.php)のオプションを意識している。

| parameter  | example       | description |
|------------|---------------|-------------|
| name       | fa-file-image | 表示したいアイコン名 (例えば[Font Awesomeで指定するクラス名](https://fortawesome.github.io/Font-Awesome/icons/)とか) |
| size       | 640x400       | 出力するSVG画像の全体のサイズ |
| region     | 96x96         | 表示するアイコン画像のサイズ (`96x96+20+20`みたいな感じで、基準位置からのオフセットも指定できる) |
| gravity    | Center        | アイコン画像の表示位置 (`NorthWest`, `North`, `NorthEast`, `West`, etc...) |
| fill       | #ffffff       | アイコン画像の色 |
| background | #287d1a       | 背景色 |

[^1]: はたして本来の意味の「“アイキャッチ”画像」として成立しているかどうかは微妙なところですが。それとこれ、Windowsの「Metro UI」のタイルに似ていますな。

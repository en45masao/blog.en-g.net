+++
date = "2015-09-30"
draft = false
title = "HDリメイク版ファイナルファンタジーVの問題点"
tags = ["reading", "game", "gamedev"]

+++

* [Doing an HD Remake the Right Way](http://www.fortressofdoors.com/doing-an-hd-remake-the-right-way/)

[Hacker News経由](https://news.ycombinator.com/item?id=10297674)で読んだ記事。HDリメイク版のファイナルファンタジーV(以下「FFV」)がリリースされたものの、肝心のグラフィックまわりの品質に[いろいろ問題がある](https://twitter.com/bill_at_zeboyd/status/647148767053934592)とのこと。記事の筆者はインディーズゲームの開発者で、自身の旧作をHDリメイクした際の経験を元に、ドット絵ベースのゲームを高解像度にリメイクする際に具体的にどうするべきであるのかを、HDリメイク版FFVの問題点を引き合いに出しつつ論じている。一般的な話だけ抜粋しておおざっぱにまとめると、以下のような感じですかね。

* まず最初に言いたいのは「オリジナル版と同じグラフィック」のモードをオプションとして用意しておこう、ということ。
	* どんなにうまくHD化したとしても、一部のユーザーは「オリジナルの方がいい」と言うものだ。
* オリジナル版のグラフィックを何倍に拡大するかは非常に重要。
	* 「1.0倍より上～2.0倍未満」の倍率は絵がつぶれて不鮮明になるので避けるべし。
	* キリのいい倍率に拡大しようとすると一般的なディスプレイ解像度に合致しない場合、内部的にはキリのいい倍率を採用して、実行時にダウンスケールするという手もある。
* スプライト等のアセットを手っ取り早くHD化する場合、実行時に拡大するのではなく、事前に拡大した画像をアセットとして持つ方がいい。
	* スプライト画像の拡大の際には、ドット絵に適した拡大アルゴリズムを選択すること。
		* 筆者のおすすめは[XBR](https://en.wikipedia.org/wiki/Image_scaling#xbr_family)。
		* 他にも[SuperEagle](https://en.wikipedia.org/wiki/Image_scaling#Super_2.C3.97SaI_and_Super_Eagle)や[HQx](https://en.wikipedia.org/wiki/Hqx)がポピュラー。
	* アルファ値の扱いには気をつける。
		* RGB部分とアルファ部分は分けてから拡大アルゴリズムを適用すること。
		* 適切なツールを使って作業する。
* もし、オリジナル版の製作時に作ったドット絵にする前の「元絵」があるなら、それをベースに作るのが一番いい結果になる。
	* 「元絵」にはアンチエイリアスがかかっていないほうが望ましい。
* 「タイル」の並びで絵を作っている場合、タイル一枚ごとに拡大処理を行ってはいけない。
	* そうすると、タイルを並べたときに境界ができてしまう。
* カットシーンでの大きなサイズのグラフィックを拡大するには[Waifu2X](https://github.com/nagadomi/waifu2x)が適している。
	* Waifu2Xは非常に処理が重いので、[GPUを使った高速版](https://github.com/lltcggie/waifu2x-caffe)がおすすめ。

詳しくは元記事を読んでください。

- - -

<blockquote class="twitter-tweet" lang="ja"><p lang="en" dir="ltr">How can a simple screenshot of FFV Steam go wrong? &#10;A few thoughts&#10;(Screenshot taken by <a href="https://twitter.com/E107Combo">@E107Combo</a> ) <a href="http://t.co/0dHCU2GtE9">pic.twitter.com/0dHCU2GtE9</a></p>&mdash; B.Stiernberg (@bill_at_zeboyd) <a href="https://twitter.com/bill_at_zeboyd/status/647148767053934592">2015, 9月 24</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

で、これが問題のリメイク版FFVの画像。タイルの継ぎ目が見えている、スプライトのドット絵がやたら平坦、全体的に画がぼやけている、などなど。たしかにこれはお世辞にも褒められたものではないな……。

この記事にも書いてあるように、以前スクウェア・エニックスがFFIやFFIVをPSPに移植した際は、スプライトのキャラにせよBGタイルの背景にせよ、きちんときれいに描きなおした形でリメイクされていたわけで、かつてはできていたことがなぜ今はできない(or やらない)のか……という気分になる。[^1] これは今作のFFVリメイクに限った話ではなく、[モバイル版FFVIのレビュー](http://kotaku.com/oh-no-square-enix-what-have-you-done-to-final-fantasy-1502268040)あたりを見るに、ドット絵ベースのFFシリーズにおけるここ最近のリメイクのクオリティは、どれも似たような体たらくらしい。

- - -

記事の中で指摘されている点の一つに、「顔グラフィックに“天野絵”をそのまま使っているけど、ゲーム内のドット絵キャラと乖離がありすぎる」という話が出ていて、思わず苦笑。初期FFシリーズにおけるこの件に関しては、我々もみんな20年以上前から同じようなことを思っていたものですよ……。

例として挙げられているこのシーンの場合、ドット絵キャラのバッツとレナの髪の色はそれぞれ茶色とピンクなのに対して、顔グラフィックの髪の色はどちらにもあてはまらないので、確かにこれは初見だとどっちのキャラなのか分からなくてもおかしくない。おまけに天野氏の描くキャラクターの絵が中性的なので、混乱に拍車をかけている感じ。

<img class="img-thumbnail" src="/post/20150930_ffv-cutscene.png">

<!--
	convert http://www.fortressofdoors.com/content/images/2015/09/ffv_cutscene.png -strip -gravity southeast -splice 0x24 -font Calibri -fill "#515151" -pointsize 16 -annotate +2+2 "Quoted from http://www.fortressofdoors.com/doing-an-hd-remake-the-right-way/" 20150930_ffv-cutscene.png
	optipng -o7 20150930_ffv-cutscene.png
-->

この件についてオリジナル版のFFVIが引き合いに出されていて、FFVIのメニュー画面では天野絵ベースの顔グラフィックが使われているが、これはオリジナルのアートワークをそのまま使っているわけではなく、ちゃんとゲーム内のドット絵キャラのデザインに合わせてアレンジしているとのこと。こうして比較すると、確かにだいぶ違いがある。

<img class="img-thumbnail" src="/post/20150930_ff6amano.png">

<!--
	convert http://www.fortressofdoors.com/content/images/2015/09/ff6amano.png -strip -gravity southeast -splice 0x24 -font Calibri -fill "#515151" -pointsize 16 -annotate +2+2 "Quoted from http://www.fortressofdoors.com/doing-an-hd-remake-the-right-way/" 20150930_ff6amano.png
	optipng -o7 20150930_ff6amano.png
-->

初期のFFシリーズでゲーム内で顔グラフィックがあったのはFFII・IV・VIだけど、自分の認識としては、IIとIVではゲームに映えるように天野絵をポップ寄りな絵にアレンジしていたのに対し、VIはそのままの絵をゲーム内に載せてきた、と思っていた。が、実はそうではなく、VIの絵も元絵をかなりアレンジしてたんですな。

- - -

この記事の筆者は当時のFFシリーズをかなりリスペクトしているようで、そのことは文章を読んでいても伝わってくる。[^2] 別の記事になるが、[自分たちの開発したインディーズゲームのメインテーマ作曲に、あの植松伸夫氏をフィーチャーした](http://www.fortressofdoors.com/nobuo-uematsu-to-work-on-defenders-quest-2/)というのだからすごい話だ。このプレスリリースの記事を出したのが4月2日なのだが、ほんとは昨日出すつもりだったけど、エイプリルフールのジョークだと思われそうなので一日待ったのだとか。

[^1]: 下請けの力量の差か、あるいは予算が少ないのか、はたまたその両方なのか。
[^2]: “a legendary game series like Final Fantasy”とか“the legendary Yoshitako Amano”とか。まあ、名前をスペルミスしているのはご愛嬌。

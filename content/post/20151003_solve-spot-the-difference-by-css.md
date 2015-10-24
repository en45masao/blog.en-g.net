+++
date = "2015-10-03"
draft = false
title = "CSSで間違い探しを解く"
tags = ["reading", "webdev"]

+++

* [Image diffing using CSS](http://franklinta.com/2014/11/30/image-diffing-using-css/)

[Hacker News経由](https://news.ycombinator.com/item?id=10320445)で読んだ記事。2枚のよく似た絵から違いを見つけ出す、いわゆる「間違い探し」を、CSSのfilter機能を使って解くというもの。

原理は単純で、一枚目の絵はふつうに表示しておいて、二枚目の絵をCSSのfilter機能の`invert(100%)`で色を反転させて、`opacity(50%)`で半透過させた状態で、一枚目の絵の上にぴったり重ねるように配置するだけ。簡単だけど、これで画像のdiffが取れるとは思いつかなかった。CSS filterは画像だけでなく普通の要素にも適用できるので、DOM要素を複製すれば「visual diff」も取れるとのこと。

こういうのを見ると自分でも試してみたくなりますな。というわけで、[難しいと評判](http://portal.nifty.com/kiji/140226163401_1.htm)の[サイゼリヤのキッズメニューの間違い探し](http://www.saizeriya.co.jp/entertainment/)をネタにやってみました。輪郭抽出みたいになってしまっているのは、元絵がJPEGなために画素単位で色成分が一致していないからか、あるいは切り出しの時点でずれているためなのか。「次の画像」ボタンを押すと、過去に出題された問題を順番に切り替えられるようにしています。

なお、元記事ではFirefox 34以前ではCSS filterが使えないために代わりに「SVG filter」を使うようにしているようだけど、Firefox 35からは(vender prefixなしで)CSS filterが使えるようなので、このサンプルではそちらを使っています。

## 元絵

<div>
	<button id="change-20151003" type="button" class="btn btn-primary btn-lg pull-right">次の画像</button>
	<a href="http://www.saizeriya.co.jp/entertainment/images/1505/body.jpg" target="_blank"><img class="source-20151003 img-thumbnail" src="http://www.saizeriya.co.jp/entertainment/images/1505/body.jpg"></a>
</div>

## diff結果

<div class="well">
	<div style="position: relative; width: 561px; margin-left: auto; margin-right: auto;">
		<div style="position: relative; overflow: hidden; width: 561px; height: 570px;">
			<img class="source-20151003" style="display: block; max-width: none; margin-left: -36px;" src="http://www.saizeriya.co.jp/entertainment/images/1505/body.jpg">
		</div>
		<div style="position: absolute; overflow: hidden; width: 561px; height: 570px; top: 0;
		            -webkit-filter: invert(100%) opacity(50%);
		            filter: invert(100%) opacity(50%);">
			<img class="source-20151003" style="display: block; max-width: none; margin-left: -599px;" src="http://www.saizeriya.co.jp/entertainment/images/1505/body.jpg">
		</div>
	</div>
</div>

<script>
jQuery(function($) {
	var imgs = [
		'http://www.saizeriya.co.jp/entertainment/images/italy/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/vegetable/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/rice/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/yaoya/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/vege/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/tomato/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/oil/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/cheese/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/1402/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/1403/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/1404/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/1409/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/1412/body.jpg',
		'http://www.saizeriya.co.jp/entertainment/images/1505/body.jpg',
	];
	$('#change-20151003').on('click', function() {
		var index = $.inArray($('.source-20151003:first').attr('src'), imgs);
		$('.source-20151003').attr('src', imgs[(index + 1) % imgs.length])
	});
});
</script>

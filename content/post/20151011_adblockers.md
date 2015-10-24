+++
date = "2015-10-11"
draft = false
title = "広告ブロッカーが原因でAjax通信に失敗する"
tags = ["reading", "webdev"]

+++

* [Adblockers as the reason of InvalidAccessError](https://wwwtech.de/articles/2015/oct/adblockers-as-the-reason-of-invalidaccesserror)

[Hacker News経由](https://news.ycombinator.com/item?id=10367217)で読んだ記事。Ajax通信にて謎のエラーが発生し、あれこれ調査した結果、広告ブロッカー(uBlock)がAjaxの通信先のURLに含まれている文字列に反応して、その通信をブロックしていたのが原因だったというお話。確かにこれは気づきにくそう。

[Hacker Newsのコメント](https://news.ycombinator.com/item?id=10367261)にて、uBlockのソース上で実際に`XMLHttpRequest.open()`をoverrideしている箇所の情報が出ているけど(<https://github.com/chrisaljoudi/uBlock/blob/master/platform/safari/vapi-client.js#L278>)、えーこんなところまで上書きしてくるのかー、という感想。このソースはSafari向けの処理なので、実際に影響を及ぼしていたのがここなのかは分からないけど……。

この記事のケースで使っていた広告ブロッカーはuBlockだったけど、たぶんこれ、他の広告ブロック系の拡張機能でも似たような感じなんでしょうな。似たような事例に出くわしたときのために、頭の片隅に置いておきたい。

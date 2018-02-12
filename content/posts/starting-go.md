---
title: "スターティングGo言語読了"
date: 2018-01-31T11:02:32+09:00
---

[スターティングGo言語](https://www.amazon.co.jp/dp/4798142417)を読み終わりました。

[lnd](https://github.com/lightningnetwork/lnd)にcontributeにしたいなと調べていたら、lndはGoで書かれており、[contribution guide](https://github.com/lightningnetwork/lnd/blob/master/docs/code_contribution_guidelines.md)のRequired Readingに[effective go](https://golang.org/doc/effective_go.html)があり、effective go読む前にもっと基礎的な部分を勉強しようと思い、スターティングGo言語を手に取りました。

<!--more-->

今まで理解できていなかったstructやslice周りがわかり、Goで書かれたコードを以前よりスムーズに読めるようになりました。

最近の暗号通貨のOSSプロジェクトは、Goで書かれているものが多い印象なのですが、なぜGoで書かれているか少しわかった気がします。静的型付けで、十分な実行速度があり、標準ライブラリに暗号ライブラリがあるのは、暗号通貨を開発するのに大きな強みだと思います。

どうしても他の言語だと、C言語で書かれたライブラリ(libsodiumなど)のbindingを使うことになりますが、Goはその点標準ライブラリをそのまま使えるのが嬉しいです。

次はeffective goを読みます。
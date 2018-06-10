---
title: "Goでサーバーサイドを書き始めて1ヶ月経った"
date: 2018-05-06T20:43:43+09:00
draft: true
---
[以前書いた](/posts/go-server-side)ように、4月からGo言語でサーバーサイドのアプリケーションを書いています。始めてから1ヶ月経ったので、忘れない内に所感を残しておきたいと思います。

- 元々Starting Goを読んでいたが、contextとgoroutine/channel周りの知識が怪しかったので、勉強した(Starting GoはGo 1.6を扱っていて、context packageはGo 1.7から導入されたから言及すらされていなかったことに後々気づいた)
- web applicationを開発するとのことで、`net/http`周りを触っていたが、`net/http`は今のところほぼ使っていない...
- microservice間の通信を実装していたので、grpcの知識が必要で勉強した。勉強にあたって、 [gRPC and REST with gRPC in practice](https://speakerdeck.com/kazegusuri/grpc-and-rest-with-grpc-in-practice
) がとても良かった
- わからない事だらけで大量に勉強することがあるが、逆に言えば伸び代が大きくて、毎日楽しい
- 独り立ちまでは中々遠い
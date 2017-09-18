---
title: "Hugo"
date: 2017-09-18T21:46:06+09:00
---

ブログを[hexo](https://hexo.io/)から[Hugo](https://gohugo.io/)に移行しました。特にGolangを勉強したいからHugoを選んだわけではなく、テーマが好みなのが多かったからです。

移行で少し面倒だったのが、

- `baseURL`の設定
- Hugoのsummariesが日本語だと自動で動かないので、`<!--more-->`を入れてあげる必要がある

の2点です。

ゆくゆくはCI通ったらdeployするようにしていきたいです。

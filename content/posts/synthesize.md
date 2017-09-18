---
layout: post
title: "synthesizeはいつ書く必要があるか"
date: 2014-08-30 17:24:49 -0700
comments: true
tags: [ios, objective-c]
---

compilerの進化に合わせて、objective-cにおける`property`の書き方も変化してきましたが、今回は`synthesize`について。

<!--more-->

## Q. `synthesize`は書く必要があるか

## A. 基本的にない

`clang`には`autosynthesis`という機能があり、何もしなくてもcompilerが

``` objective-c
@synthesize propertyName = _propertyName;
```

`synthesize`してくれる

## cf
http://stackoverflow.com/questions/19784454/when-should-i-use-synthesize-explicitly

## 所感
9/9の発表に合わせ、そろそろ`swift`に手をつけようかなと考えています。

---
layout: post
title: "propertyの属性"
date: 2014-05-04 02:47:35 -0700
comments: true
tags:
---

propertyを宣言する場合に、どの属性を指定すべきかまとめたものです。

<!--more-->

## primitive(非オブジェクト型): assign又は省略
NSIntegerやfloatなどのprimitive型は`assign`を指定するか、又は省略することができます。primitive型の場合、デフォルトが`assign`になっているからです。

## Mutableなサブクラスがあるクラス
`NSString`や`NSArray`の様に、mutableなサブクラスがある場合、`copy`を指定します。

```objective-c
NSMutableString *string = [NSMutableString  stringWithString @"abc"];

MyClass *obj = [[MyClass alloc] init];
obj.str = string;

[string appendString:@"def"];
// if strong, obj.str will be @"abcdef"
// if copy, obj.str will be @"abc"
```

`property`の属性に`copy`を指定するか、`strong`を指定するかで値が変わりますね。`NSString`というimmutableなクラスのインスタンスなのに、知らぬ所で変更が加えられると、想定しない挙動を起こし、不具合の元になります。

## block
`block`もcopyを指定します。`block`は本来stackに存在するので、スコープを抜けると解放されてしまいます。スコープを抜けた後も参照したい場合は、`[block copy]`をすることによって、ヒープにblockを作成します。

## delegate
`delegate`は循環参照を避けるために、`weak`を指定します。

## その他のオブジェクト
`strong`を指定するか、省略します。オブジェクト型はデフォルトで`strong`が指定されるので、省略する事ができます。


## 参考
http://qiita.com/uasi/items/80660f9aa20afaf671f3

http://stackoverflow.com/questions/387959/nsstring-property-copy-or-retain

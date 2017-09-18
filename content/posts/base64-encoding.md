---
layout: post
title: "objective-cでNSStringをbase64 encode"
date: 2014-04-01 23:49:45 -0700
comments: true
tags: [objective-c, base64]
---


ios7から、`NSData`に`base64EncodedStringWithOptions:`というメソッドが新たに追加され、base64 encodingが楽になりました。しかも、ios7より以前のバージョン用に、それまでprivateだった`base64Encoding`というメソッドがpublicになりました。

<!--more-->

`NSString`をbase64でencodeした`NSString`に変換するには、以下のようにやります。

``` objective-c
NSString *string = @"foo";
NSData *data = [string dataUsingEncoding:NSUTF8StringEncoding];
NSString *base64EncodedString = nil;

if ([data respondsToSelector:@selector(base64EncodedStringWithOptions:)])
{
    //ios 7
    base64EncodedString = [data base64EncodedStringWithOptions:0];
}
else
{
    // pre ios 7
    base64EncodedString = [data base64Encoding];
}
```

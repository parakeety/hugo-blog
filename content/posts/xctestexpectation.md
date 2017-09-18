---
layout: post
title: "XCTestExpectation"
date: 2014-10-06 23:16:00 -0700
comments: true
tags: [XCTestExpectation, XCTest, iOS]
---

## 概要

`XCTestExpectation`とは、xcode6にて追加された非同期テスト用のAPIです。
これまで、非同期な処理のテストをするのにGCDのAPIを使ったり、[TRVSMonitor](https://github.com/travisjeffery/TRVSMonitor)等のライブラリを使う必要がありましたが、今後は`XCTestExpectation`を使うことができます。

<!--more-->

## 使い方

```objective-c
// 1. XCTestExpectationのインスタンスを生成。複数のXCTestExpectationを生成しても良い
XCTestExpectation *expectation = [self expectationWithDescription:@"callback called"];

// 2. 非同期な呼び出し内で`[expectation fulfill]`を呼ぶ
[SomeClass callback:^(NSString *string, NSError *error) {
    XCTAssert(...);

    // [expectation fulfill]が呼ばれると、3.のwaitForExpectationのcallbackが呼ばれる
    [expectation fulfill];
}];

// 3. `[expectation fulfill]`が呼ばれるか、タイムアウトになるまで`NSRunLoop`
// を回し続けた状態で待機する
[self waitForExpectationsWithTimeout:1 handler:nil];

```

## cf
http://qiita.com/nomadmonad/items/d2c283f9b9ad33a2b32a
http://sssslide.com/speakerdeck.com/nomadmonad/new-features-of-xctest-on-xcode-6
http://nshipster.com/xctestcase/

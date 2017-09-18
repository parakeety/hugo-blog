---
layout: post
title: "XCTestで書いたUnit Testのリファクタリングを試みた"
date: 2014-03-28 16:19:32 -0700
published: true
comments: true
tags: [objective-c, XCTest, Unit Test]
---

XCTestで書いたUnit testのrefactoringを試みたのですが、個人的にすっきりする方法が見つかりませんでした。

<!--more-->

例として、`name`というpropertyと、`validateName:error:`というmethodを持った`Person`というClassがあったとします。

``` objective-c Person.h
@interface Person : NSObject
@property(nonatomic, copy) NSString *name;
- (BOOL)validateName:(id *)ioValue error:(NSError * __autoreleasing *)outError;
```

``` objective-c Person.m
#import "Person.h"

@implementation Person
- (BOOL)validateName:(__autoreleasing id *)ioValue error:(NSError *__autoreleasing *)outError
{
    if (*ioValue == nil)
    {
        if (error)
        {
            NSDictionary *userInfo =
            @{
              NSLocalizedDescriptionKey: @"Invalid name.",
              NSLocalizedFailureReasonErrorKey: @"Name should not be nil."
            };
            *error = [NSError errorWithDomain:PKTErrorDomain code:PKTInvalidName userInfo:userInfo];
        }

        return NO;
    }

    return YES;
}
@end
```

`validateName:error:`は`name`が`nil`の場合に`NSError`を作成して返します。

`validateName:error:`が求めている`NSError`を返すかどうかテストしましょう。

## (1) 愚直に各attributeを比較する

``` objective-c ParakeetTest.m
- (void)testNilNameReturnsExpectedError
{
    NSError *error = nil;
    NSString *name = nil;
    [_person validateName:&name error:&error];

    XCTAssertNotNil(error, @"Error object should not be nil.");
    XCTAssertEqualObjects([error domain], PKTErrorDomain, @"Error domain should be PKTErrorDomain.");
    XCTAssertEqual([error code], PKTInvalidName, @"Error code should be PKTInvalidName.");
    XCTAssertEqualObjects([error localizedDescription], @"Invalid name.", @"It should return expected localized description.");
    NSDictionary *userInfo = [error userInfo];
    NSString *failureReason = [userInfo objectForKey:NSLocalizedFailureReasonErrorKey];
    XCTAssertEqualObjects(failureReason, @"Name should not be nil.", @"It should return expected failure reason.");
}
```

`NSError`のattributesをひたすら期待通りか`assertion`で確認していますね。
Unit testの原則として、一つのテストにつき、assertionが一つなのが望ましいのですが、ここではAssertionが複数埋め込まれています。[Assertion Roulette](http://xunitpatterns.com/Assertion%20Roulette.html)と呼ばれ、あまり推奨されない書き方です。  

test codeのrefactoringをしてみましょう。

## (2) custom assertionを関数で実装
Assertion Rouletteの解決方法の一つとして、[custom assertion](http://xunitpatterns.com/Custom%20Assertion.html)というのがあります。
そこでcustom assertionを作ってみましょう。

``` objective-c CustomAssertion
static void PKYAssertNSErrorObjects(NSError *error, NSError *expected)
{
    XCTAssertEqualObjects([error domain], [expected domain],@"Domains should be the same");
    XCTAssertEqual([error code], [expected code], @"Codes should be the same");
    XCTAssertEqualObjects([error localizedDescription], [expected localizedDescription], @"localized descriptions should be the same");
    NSString *failureReason = [[error userInfo] objectForKey:NSLocalizedFailureReasonErrorKey];
    NSString *expectedFailureReason = [[expected userInfo] objectForKey:NSLocalizedFailureReasonErrorKey];
    XCTAssertEqualObjects(failureReason, expectedFailureReason, @"Failure reasons should be the same");
}
```

NSErrorを比較していた部分を一つの関数に移しました。

これを使って先ほどのコードを書き直すと
``` objective-c
- (void)testNilNameReturnsExpectedError
{
    NSError *error = nil;
    NSString *name = nil;
    [_person validateName:&name error:&error];

    NSDictionary *expectedUserInfo = @
    {
        NSLocalizedDescriptionKey: @"Invalid name.",
        NSLocalizedFailureReasonErrorKey: @"Name should not be nil."
    };
    NSError *expected = [NSError errorWithDomain:PKTErrorDomain code:PKYInvalidName userInfo:expectedUserInfo];
    PKYAssertNSErrorObjects(error, expected);
}
```

とすっきり書ける事になります。
ところがcustom assertionがsyntaxエラーを吐いております。

{% img /images/xctest1.png %}

"Use of undeclared identifier 'self'"と表示されていますね。
self??と疑問に感じ、`XCTAssert`を探っていくと全ての`XCTAssertion`は`_XCTRegisterFailure`を呼び出している事がわかります。

`_XCTRegisterFailure`を見てみましょう。

``` objective-c XCTestAssertionsImpl.h mark:4
XCT_EXPORT void _XCTFailureHandler (XCTestCase *test, BOOL expected, const char *filePath, NSUInteger lineNumber, NSString * condition, NSString * format, ...) NS_FORMAT_FUNCTION(6,7);

#define _XCTRegisterFailure(condition, format...) \
({ \
    _XCTFailureHandler(self, YES, __FILE__, __LINE__, condition, @"" format); \
})

```

`_XCTRegisterFailure`は`_XCTFailureHandler`を呼び出していますね。
`_XCTFailureHandler`の一つ目の引数のtypeはXCTestCaseなのですが、`_XCTRegisterFailure`は、そこに`self`を渡しています。

つまり、`XCTAssertion`を呼ぶ場合、`self`に`XCTestCase`のinstanceが入っていないといけない様です。

## (3) Custom assertionをinstance method
では、custom assertionをinstace methodとして実装してみればどうでしょうか。

``` objective-c
- (void)customAssertError:(NSError *)error expected:(NSError *)expected
{
    XCTAssertEqualObjects([error domain], [expected domain], @"Error domain should be the same.");
    XCTAssertEqual([error code], [expected code], @"Error code should be the same.");
    XCTAssertEqualObjects([error localizedDescription], [expected localizedDescription], @"It should return expected localized description.");
    NSDictionary *userInfo = [error userInfo];
    NSString *failureReason = [userInfo objectForKey:NSLocalizedFailureReasonErrorKey];

    NSDictionary *expectedUserInfo = [expected userInfo];
    NSString *expectedFailureReason = [expectedUserInfo objectForKey:NSLocalizedFailureReasonErrorKey];
    XCTAssertEqualObjects(failureReason, expectedFailureReason, @"It should return expected failure reason.");
}
```
method名がtestで始まると、テストメソッドとして認識されてしまうため、testで始まらない名前をつけます。
今回は構文エラーが表示されませんね。このcustom assertionを使って、テストコードを書きなおしてみましょう。

``` objective-c
- (void)testNilNameReturnsExpectedError
{
    NSError *error = nil;
    NSString *name = nil;
    [_person validateName:&name error:&error];

    NSDictionary *expectedUserInfo = @
    {
        NSLocalizedDescriptionKey: @"Invalid name.",
        NSLocalizedFailureReasonErrorKey: @"Name should not be nil."
    };
    NSError *expected = [NSError errorWithDomain:PKTErrorDomain code:PKYInvalidName userInfo:expectedUserInfo];
    [self customAssertError:error expected:expected];
}
```

すっきりしましたね。しかし、この方法にも欠点がないわけではありません。

1. 複数のテストメソッドからcustom assertionを呼び出している
2. 複数のテストメソッドが、custom assertion内のassertionでこける

この2点を満たす場合、各テストがなぜ通らなかったのか非常にわかりづらく、デバッグしづらいです。

## (4) NSErrorを比較
「各attributeを比較しているが、NSError同士を比較たらどうか?」と疑問に思われるかもしれません。
NSErrorでは`isEqual:`がoverrideされているかどうかはドキュメントに書かれておらず、どう比較しているのかはわかりませんが、attributesが等しいかどうかは`isEqual:`でも判別できます。

しかし、テストが落ちた際、(3)よりも、なぜテストが落ちたがわかりづらいという欠点があります。

{% img /images/xctest2.png %}

マウスカーソルを失敗したassertionの上に動かさなければならず、一定時間が経つと、hoverで表示されている情報がフェードアウトします。表示されている情報も見やすいとは言えません。


## まとめ
(1)重複も多いが、テストが失敗した時にどこが失敗したかすぐわかり、デバッグがしやすい  
(2)selfにxctestcaseのinstanceを代入する必要がある  
(3)複数のテストがcustom assertion内のassertionで失敗した時に、失敗した原因が一目瞭然ではない  
(4) (3)よりも失敗原因がわかりづらい上に見づらい  

(1)か(3)のどちらかで行こうかなと思ってはいますが、何か他に良い方法はないものか..

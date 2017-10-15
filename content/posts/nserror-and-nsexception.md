---
title: "NSErrorとNSException"
date: 2014-12-30 17:23:39 -0800
tags: [iOS, Objective-c, Swift, NSError, NSException]
---

Cocoaには、エラーが起きた時の対処方法として、`NSError`と`NSException`があります。

<!--more-->

## `NSError`と`NSException`の使い分け
Non-recoverableなケースのみ`NSException`を使い、それ以外は原則として`NSError`を使います。

`NSException`を使うケースとしては、methodの引数がinvalidな時(コールバックが必須なのに、nilだったなど)です。引数のチェックに私はよく`NSParameterAssert`を使っています。

```objective-c
typedef void (^CompletionBlock)(NSArray *array, NSError *error);
- (void)someMethod:(CompletionBlock)callback
{
  NSParameterAssert(callback);

  // 処理
}
```

## `NSError`の使い方
`NSError`に含まれる情報は大きく3つあります。

1. domain
2. code
3. userInfo

domainは文字列、codeはenumで定義される事が多く、どの領域でエラーが起こったか識別するのに利用します。userInfoは`NSDictionary`のインスタンスで、`localizedDescription`というメソッドを呼ぶと、エラーに関する記述が含まれている文字列を返します。この文字列は、エラーが起きた際に、ユーザーに見せる情報としてよく使います。

```
// objective-c
if (error) {
  NSLog(@"%@", [error localizedDescrition]);
}

// swift
if let newError = error {
  error.localizedDescription
}
```

## `NSError`の作り方
Cocoaではなく自分のアプリ/ライブラリ由来の`NSError`を定義したい場合は、`errorWithDomain:code:userInfo:`を使います。

```objective-c
// objective-c
NSString *const ABCErrorDomain = @"com.example.abc";
[NSError errorWithDomain:ABCErrorDomain
                    code:1
                userInfo:nil];
```

```swift
// swift
let ABCErrorDomain = "com.example.abc"
NSError(domain: ABCErrorDomain, code: 1, userInfo: nil)
```

domainに関しては、[リバースドメイン形式が推奨](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/ErrorHandling/ErrorHandling.html)されています。

objective-cで書く場合、専用のheaderファイルとimplementationファイルを用意し、そこにNSErrorに関する定義をまとめます。

```objective-c
// ABCError.h
FOUNDATION_EXPORT NSString *const ABCErrorDomain;
typedef NS_ENUM(int, ABCErrorCode) {
  ABCSomeError
};

// ABCError.m
NSString *const WPYErrorDomain = @"com.example.abc";
```

## cf
- http://nshipster.com/nserror/
- https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Exceptions/Exceptions.html
- http://stackoverflow.com/questions/11100951/nsexception-and-nserror-custom-exception-error
- http://stackoverflow.com/questions/8396326/exception-handling-in-entire-application

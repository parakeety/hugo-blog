---
title: "objective-cにおけるprotectedなpropertyの定義方法"
date: 2014-05-18 21:39:37 -0700
tags: [Objective-c, iOS]
---

privateな`property`は`class extension`を使えば良いですが、`protected`、即ち`subclass`からもアクセスできる`property`が欲しい場合どうするか。

<!--more-->

## Step 1: protectedなインスタンス変数をヘッダーで宣言

```objective-c
// SuperClass.h
@interface SuperClass: NSObject
{
  NSObject *_variable;
}
@end
```
superclassのヘッダーで、`protected`なインスタンス変数を宣言します。


## Step 2: SuperClassにもう一つヘッダーファイルを宣言
```objective-c
//SuperClass_protected.h
#import "SuperClass.h"

@interface SuperClass ()
@property (nonatomic , readonly) NSObject *variable;
@end
```

`SuperClass.h`を`import`し、`SuperClass`の`class extension`を使って、先ほど宣言したインスタンス変数に対応する`property`を`readonly`を指定して、宣言します。

## Step 3: 実装ファイル(.m)でインスタンス変数と`property`をsynthesize


```objective-c
// SuperClass.m
#import "SuperClass_protected.h"
@implementation SuperClass
@synthesize ivar = _ivar;
@end
```

## 参考
http://stackoverflow.com/questions/11047351/workaround-to-accomplish-protected-properties-in-objective-c

http://stackoverflow.com/questions/19321001/creating-properties-only-visible-to-subclass-in-objective-c

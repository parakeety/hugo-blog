---
layout: post
title: "private property vs privateなインスタンス変数"
date: 2014-05-11 03:46:29 -0700
published: true
comments: true
tags: [objective-c]
---

外部から隠蔽したい状態を保持するには、privateな`property`とprivateなインスタンス変数と2通りのやり方があります。

<!--more-->

## private property
priavateな`property`は`class extension`を使って実装します。

```objective-c
// MyClass.m
@interface MyClass () //class extension
@property(nonatomic, strong) NSString *name;
@end
```

イニシャライザでは`_property`を、それ以外では`self.property`を使って参照します。

```objective-c
- (instancetype)init
{
  if(self = [super init])
  {
    _name = @"test";
  }
  return self;
}

- (void)otherPlaces
{
  self.name = @"test";
}
```

## privateなインスタンス変数
```objective-c
// MyClass.m
@implementation
{
  NSString *_name;
}

@end
```

どこで参照するにも`_ivar`を使います。

```objective-c
- (instancetype)init
{
  if(self = [super init])
  {
    _name = @"test";
  }
  return self;
}

- (void)otherPlaces
{
  _name = @"test";
}
```

## どちらが良いか?

1. インスタンス変数だと、代入しても、KVOが呼ばれない
2. `strong`以外の修飾子を指定する場合、propertyの方が楽. i.e)copy

以上からプライベートな状態を保持したい場合、private propertyの方が良いと思います。


## 参考
http://stackoverflow.com/questions/19982735/objective-c-private-variables-vs-private-properties

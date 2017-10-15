---
title: "switch"
date: 2014-03-31 22:21:28 -0700
tags: [Objective-c]
---

### objective-cのswitchは整数値しか判定できない

objective-cにおける`switch`の条件式は、integral(整数値. intなど)しか受け付けません。なので、以下の様に`NSString`のインスタンスを条件式に渡すと、構文エラーになります。

<!--more-->

``` objective-c
NSString *country = @"Japan";
switch(country){
  case @"Japan":
    break;
  case @"USA":
    break;
  case @"Russia":
    break;
  default:
    break;
}
```



ではどうするかと言うと、`enum`を使って実装しました。

```
typedef NS_ENUM(int, Country){
  USA,
  Japan,
  Russia
};

NSString *country = @"Japan";
NSArray *countries = @[@"USA", @"Japan", @"Russia"];
int index = [countries indexOfObject: country];
switch(index){
  case USA:
    // do something
    break;
  case Japan:
    break;
  case Russia:
    break:
  default:
    break;
}

```

`enum`と`NSArray`の各Itemの対応を常に保つのが大変ですね。
`NSDictionary`のkeyとvalueで一緒に保管するのも良いと思います。
`isEqual:`で比較してくれると一番楽なのですが...



### 参考
------
Stack Overflow: Can Objective-C switch on NSString?[http://stackoverflow.com/questions/8161737/can-objective-c-switch-on-nsstring](http://stackoverflow.com/questions/8161737/can-objective-c-switch-on-nsstring)

---
title: "equality"
date: 2014-03-28 13:41:01 -0700
tags: [iOS, Objective-c]
---

#TODO

equal
identical

NSObject isEqual

Container class: deep comparison


overriding isEqual
- isEqualClassName
- call isEqualClassName in isEqual
- override hash

if isEqual: determines that two objects are equal, they must have the same hash value

```
NSString *a = @"a";
NSString *b = @"b";
a == b is true
```

use isequaltoString
never use == for comparing strings. string interning

参考記事
http://nshipster.com/equality/
https://developer.apple.com/library/ios/documentation/general/conceptual/DevPedia-CocoaCore/ObjectComparison.html

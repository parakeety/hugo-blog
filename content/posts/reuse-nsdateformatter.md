---
title: "NSDateFormatterの再利用"
date: 2014-10-27 11:40:17 -0700
tags: [iOS, NSDateFormatter, Objective-c]
---
`NSDateFormatter`を含む`NSFormatter`クラスは、生成コストが高いのでなるべく再利用します。

<!--more-->

```objective-c
+ (NSDateFormatter *)dateFormatter
{
    static NSDateFormatter *_dateFormatter = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        _dateFormatter = [[NSDateFormatter alloc] init];
        [_dateFormatter setDateFormat:@"YYYY/MM/dd"];
    });
    return _dateFormatter;
}

- (void)aMethod
{
    NSDateFormatter *formatter = [[self class] dateFormatter];
}
```

## cf
- http://nshipster.com/nsformatter/
- http://stackoverflow.com/questions/8832768/why-is-allocating-or-initializing-nsdateformatter-considered-expensive

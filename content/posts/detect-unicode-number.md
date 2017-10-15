---
title: "文字からUnicode numberを識別する"
date: 2014-06-30 00:26:10 -0700
tags: [unicode, python]
---

日本語だと横棒がハイフンなのか、ダッシュなのか、全角マイナスなのかの識別が目視ではしづらいです。そこで、以下のpythonスクリプトを実行すれば、Unicode numberを返してくれます。

```python
# detect ―(0x2015)
print hex(ord(u'―'))
```

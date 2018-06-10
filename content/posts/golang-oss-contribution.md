---
title: "Goで書かれたOSSにPR送る時のimport問題"
date: 2018-05-22T19:58:03+09:00
---

githubでhostされているOSSにPRを送る時の良くある手順としては、

1. forkする
2. forkしたrepositoryをcloneする
3. 作業
4. forkにpush
5. PR送る

があると思いますが、Goで書かれているOSSの場合、forkしたコード内でfork元の方のsubpackageをimportして動かない事がありました。

<!--more-->

```go
// fork元
import "github.com/original/subpackage"
```

```go
// fork
import "github.com/original/subpackage" // 本当は"github.com/fork/subpackage"を指して欲しい
```

## 解決策
[こちらの記事](http://blog.campoy.cat/2014/03/github-and-go-forking-pull-requests-and.html
)に書いてあるように、

1. forkする
2. originalの方のrepositoryをcloneする
3. 2でcloneしたrepositoryに、1のrepositoryをremoteとして追加する
4. 2でcloneしたrepoistoryで作業
5. 3で追加したforkにpush

でやるのが良さそうです。
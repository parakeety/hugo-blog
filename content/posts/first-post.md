---
title: "Octopress始めました。"
date: 2014-03-27 23:07:15 -0700
tags:
---

他のブログサービスなども検討しましたが、

1. 無料
2. markdownが使える
3. 好きなエディタで文章を書く事ができる

以上の3点が自分にとっては大きく、octopressにしました。

<!--more-->

vimで日本語を使った事がないので、フォントがおかしいことや、bundle execを一々付けないといけない事とか、自動preview更新などのworkflowの改善は追々やろうと思います。

ブログの内容ですが、主にiOSやjavascript/coffeescriptに関する記事が多くなると思います。



以下はoctopressに関する自分用のメモです。

## create new post

```
bundle exec rake new_post['post title']
```

## preview

```
bundle exec rake preview
```

http://localhost:4000

## publish

```
bundle exec rake generate
bundle exec rake deploy
```

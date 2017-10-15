---
title: "開発ツール"
date: 2014-07-05 02:42:35 -0700
tags: [z, tig, ghq, peco]
---

仕事で色々inputはしているのだが、それをきれいにまとめる、足りない分を調査する時間が中々取れていない。jsでの依存関係解決の話、moduleの話(commonjs, AMD, UMD)、[browserify](https://github.com/substack/node-browserify)とネタは溜まっているので、追々記事にしていきたいと思います。

<!--more-->

今日は最近導入したツールについて紹介します。

## [Z](https://github.com/rupa/z)
ディレクトリの移動履歴を記録して、よく行くディレクトリに簡単に移動できます。

例えば、`~/code`に現在いて、`~/code/js/coffeelint-prefer-english-operator`に移動したいとします。

`z lint`

とタイプするだけで、移動する事ができます。どこにいても、良く使うディレクトリに簡単に移動できるのでとても便利です。

#### cf
[z.shでよく行くディレクトリに手軽に移動する](http://qiita.com/yoshikaw/items/38d3008ac7d0b19b4805)


## [tig](https://github.com/jonas/tig)

{% img /images/tig.png %}

git viewerです。git blameや、行単位でstage/unstageの指定の機能がとても便利です。元の色が気に入らなかったので、[stephenway/monokai.terminal](https://github.com/stephenway/monokai.terminal)を入れています。`.tigrc`で細かい設定ができます。

#### cf
[git? tig!](http://blogs.atlassian.com/2013/05/git-tig/)
[tigでgitをもっと便利に！ addやcommitも](http://qiita.com/suino/items/b0dae7e00bd7165f79ea)


## [ghq](https://github.com/motemen/ghq)
githubのrepositoryを実行したり、中のコードを読むために、localにcloneする事はよくあることです。次第に大量のrepositoryが溜まっていき、見つけるに苦労する事があったのですが、ghqを使うと、手元のrepositoryをうまく管理してくれます。具体的には、`$GOPATH/src`あるいは`gitconfig`の`ghq.root`で設定したパスにcloneしてくれます。

`ghq list`で一覧、`ghq look`で選択したレポジトリのディレクトリに移動します。

#### cf
http://motemen.hatenablog.com/entry/2014/06/01/introducing-ghq
http://r7kamura.github.io/2014/06/21/ghq.html


## [peco](https://github.com/peco/peco)
incrementalなgrep。とてもUNIXライク。ghqと組み合わせると、とても協力です。

#### cf
[peco、ghq、gh-openの組み合わせが捗る](http://webtech-walker.com/archive/2014/06/peco-ghq-gh-open.html)

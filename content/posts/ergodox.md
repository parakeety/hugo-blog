---
title: "Ergodox購入"
date: 2018-03-04T15:24:21+09:00
---

今までMacbook Proで作業していましたが、ひどい肩こりに悩まされたため、セパレート式のメカニカルキーボードであるErgodoxを購入しました。Ergodoxを使い始めてから2週間ほど経ちますが、肩こりが劇的に改善されて、少々高かったですが買って良かったなと満足しています。

<!--more-->

## 購入
- Ergodox EZで購入
- 1/25に注文して、実際に届いたのが2/16
- 黒の無刻印
- Cherry MX Slient Red

## キーマップをDocker使ってbuild
デフォルトのキーマップが扱いづらいため、Macbook Proに近いキーマップにとりあえず変更しました。
かなり頻繁にキーマップを変更するため、Dockerを使ってビルドしていました。

1. `https://github.com/qmk/qmk_firmware` をclone
2. `keyboard/ergodox_ez/keymaps`に`parakeety`というdirectoryを作成
3. `keyboard/ergodox_ez/keymaps/default/keymap.c`を2で作ったdirectory配下にcopy
4. 3でcopyしたkeymap.cを色々変更
5. `docker run -e keymap=parakeety -e keyboard=ergodox_ez --rm -v $('pwd'):/qmk:rw edasque/qmk_firmware`
6. repositoryのroot directoryに`ergodox_ez_parakeety.hex`が作成されているので、Teensy Loader Applicationを使って流し込む

## キーマップ
macbook proになるべく合わせたのですが、Ergodoxは右側のキーが足りず、いくつかのキー(`-`, `+`, `[`, `]`など)の配置に困りました。結局Ergodoxだけにある、人差し指でタイプするキーに割り当てました。また、キーが縦に直列に並んでいて`C``V``N``M`を良くtypoしましたが、1週間も経つと慣れました。
`Cmd + Shift`をbrowserのtab移動や、XCode使う際に多用するので、親指で押すキーの一つに`Cmd + Shift`を割り当てたのですが、かなり使いやすくて気に入っています。DeleteはCtrl + hで代用できそうで、消そうかなと今は考えています。

## 買ってみて
最初の1週間はtypoが多く、かなりタイピング速度が落ちましたが、最近は少しずつ慣れてきました。何より肩こりが解消されたので、大変満足しています。
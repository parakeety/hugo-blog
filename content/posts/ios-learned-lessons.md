---
title: "アプリやiosライブラリを作って感じたこと"
date: 2014-05-25 02:32:41 -0700
tags: [iOS, Objective-c, xctest]
---

## Viewは最初の設計でうまくいかない事が多い
iOSにおけるViewは様々な制約(subclassしてはいけない、subclassしないといけない)があり、最初にしっかり設計しても、ハウルの動く城の様につぎはぎになってしまう事が多いです。

<!--more-->

1. sketch, mockでUI/UXを形にする
2. 汚くていいからプロトタイプを作る
3. ある程度完成に近づいてきたら、その時点で設計し直して、作り直す

というのが最終的には一番無駄が少ないのかなと感じています。


## XCTestは避けたい
コンテキスト毎にsetupが設定できないため、
- 重複が多くなる
- 一番重要な部分(input,output)がぱっと見わからなくなる
点やヘルパーを書けないのが辛い。

ただ、`Cmd + U`の恩恵は大きいのは実感します。[Kiwi](https://github.com/kiwi-bdd/Kiwi)は`BDD`風に書け、`Cmd + U`も使えるみたいなので、テストフレームワークは`Kiwi`に移行しようかなと考えています。

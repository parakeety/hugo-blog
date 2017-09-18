---
layout: post
title: "cocoapods-trunk"
date: 2014-09-18 13:22:51 -0700
comments: true
tags: [ios, cocoapods, cocoapods-trunk]
---

以前[cocoapods specへの登録方法](http://parakeety.github.io/blog/2014/04/27/registration-process-to-cocoapods-spec/)を紹介しましたが、[Cocoapods trunk](http://blog.cocoapods.org/CocoaPods-Trunk/)の登場によって、やり方が変わったので、新しい方法を紹介します。

<!--more-->

## 用語説明
### cocoapods
`cocoapods`はiOSにおける依存関係管理ツールです。GitHubに公開されているオープンソースなソフトウェアを簡単にプロジェクトに組み込む事ができます。

### pod
`cocoapods`に登録されている各種ライブラリを、`pod`と呼びます。

### cocoapods spec
`spec`とは、あるバージョンにおける`pod`のメタ情報を記述したファイルです。ライブラリのバージョンの数だけ、`spec`があります。

### cocoapods trunk
コマンドラインから`pod`の公開や更新を可能にしたウェブアプリケーションです。

## 流れ一覧
0. cocoapodsをインストールする
1. cocoapods trunkに登録する
2. podspecなどの必要なファイルを揃える
3. cocoapods trunkを使ってlibraryをpushする

## 説明
### 1. cocoapodsをインストールする
Macをお使いの方は、

```sh
$ gem install cocoapods
```
とコマンドを入力すると、インストールできます。

### 2. cocoapods trunkに登録する
`pod trunk register`コマンドを使い、登録します。

```sh
$ pod trunk register YOUR_EMAIL_ADDRESS 'YOUR NAME'
```

YOUR_EMAIL_ADDRESSには、自分のemailアドレスを、YOUR NAMEには自分の名前を入れて下さい。


### 3. podspecを作成する
XCodeプロジェクトのルートディレクトリに移動し、

```sh
$ pod spec create PROJECT_NAME
```

コマンドを実行します。PROJECT_NAMEには、プロジェクトの名前を入力して下さい。
`PROJECT_NAME.podspec`というファイルが生成されるので、それに必要な情報を記入します。

生成されたpodspecには、コメントに書き方の例が書いてあるので、そちらと[Podspec Syntax Reference](http://guides.cocoapods.org/syntax/podspec.html)を参考しながら書くのをお勧めします。

### 4. cocoapods trunkを使ってlibraryをpushする
最後にlibraryの公開です。

```sh
$ pod trunk push PROJECT_NAME.podspec
```
このコマンドはpodspecをlintして、lintが通った場合、公開までやってくれるコマンドです。

## cf
http://guides.cocoapods.org/making/getting-setup-with-trunk.html

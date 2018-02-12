---
title: "xcenvで複数のXCodeを管理"
date: 2017-11-17T19:28:59+09:00
draft: true
---
XCodeのversionとSwiftのversionは紐付いており、プロジェクトによってはSwiftの新versionに対応していない、依存ライブラリをまだ更新していないなど、複数のversionのXCodeを手元に置いておきたいことがiOS開発をしていると多々あります。

複数のXCodeを手元で共存させたい時は、`XCode9.1.app`のように`XCode.app`をrenameして、XCodeのversionを切り替えたい時は、`xcode-select`で使いたいversionのXCodeへのpathを設定しないといけないです。

違うversionのXCodeの切り替えが手間だなと思っていたのですが、[xcenv](https://github.com/xcenv/xcenv)を使うと、各プロジェクト毎に設定したversionのXCodeを使ってくれます。

## 1. インストール
- `brew install xcenv`
- shell profileに`eval "$(xcenv init -)"`を追加(fishの場合は、`status --is-interactive; and . (xcenv init -|psub)`)

## 2. プロジェクトのディレクトリに`.xcode-version`を作成
プロジェクト毎に`.xcode-version`ファイルを作成します。使いたいXCodeのversionを書くだけで大丈夫です。 i.e) `9.1`
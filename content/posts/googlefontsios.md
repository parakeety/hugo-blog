---
title: GoogleFontsiOS
date: 2015-08-07 22:06:58
tags: [iOS, 'Google Fonts', GoogleFontsiOS, 'Google Fonts iOS']
---

[GoogleFontsiOS](https://github.com/parakeety/GoogleFontsiOS)というライブラリを公開しました。

<!--more-->

iOSでは標準で用意されているfont以外にも、font fileを用意すればそのfontを利用することができます。
Webでお馴染みの[Google Fonts](https://www.google.com/fonts)がiOSのアプリでも使えるか調べた所、[各fontのライセンスに則れば利用可能](http://daveaddey.com/?p=916)ということがわかりました。

各fontはCocoaPodsのPod化されておらず、[手動でfont fileをdrag & dropし、info.plistに使用するfontを追記する作業](http://codewithchris.com/common-mistakes-with-adding-custom-fonts-to-your-ios-app/)が必要だと判明し、毎回プロジェクト毎に手作業でやるのは面倒なため、Google Fontsのfont全てをCocoaPodsのsubspecにしたのが本ライブラリです。


## ライブラリの説明
### Install
Podfileに`GoogleFontsiOS/<使用したいFont Family>`と書いて、`pod install`すれば使えます。`OpenSans`を使いたかったら

```
pod 'GoogleFontsiOS/OpenSans'
```

と書けばOKです。

### 使い方
UIFontのカテゴリとして定義されているので、

```
UIFont * font = [UIFont openSansLightFontOfSize:14.0f];
```

の様に呼ぶ事ができます。


## 実装
### 達成したかったこと
1. 手動でfont fileをdrag&dropしたり、`info.plist`をいじるのを避ける
2. 必要なfontのみ組み込む
3. 必要になった時にfontをloadする

### fontのdynamic load
[この記事](http://www.mokacoding.com/blog/sharing-assets-with-cocoapods-resource-bundle-and-dynamically-loaded-fonts/)を参考に、font fileは`bundle`で、font fileは実行時にロードするようにしました。これで達成したかったことの1.と3.はクリア

### subspec
Google Fontsは現在800ほどfont familyがあり、使用しないfontを含めるのは非効率なので、各font family毎にsubspecを作る事にしました。subspecを手動で全部書くのは手間なので、Google Fontsのfontごとに分類されたディレクトリ内の`METADATA.json`をparseして必要な情報を抽出し、それを元に`podspec`をコードで生成するようにしました。これで達成したかったことの2.もクリア。

### interface
標準搭載のfontと同じinterfaceで扱える様に、各fontをUIFontのカテゴリとして定義。これも手動で書くのは手間なので、コードで自動生成。


## 所感
自動生成にrubyを使ったのですが、rubyの基本的な文法しかわかっておらず、「これで良いのだろうか...」と悩みながら書いていました。rubyは書いていて楽しかったので、今後勉強する度に少しずつ自動生成部分のコードを改善してきたいです。


## cf
http://www.mokacoding.com/blog/sharing-assets-with-cocoapods-resource-bundle-and-dynamically-loaded-fonts/
http://guides.cocoapods.org/making/using-pod-lib-create.html

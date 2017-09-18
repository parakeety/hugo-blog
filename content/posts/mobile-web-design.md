---
layout: post
title: "モバイル向けのウェブサイトを作る上でのTips"
date: 2014-06-15 01:11:03 -0700
comments: true
tags: [mobile, web, safari]
---

## meta tagを追加する
`<head>`内に以下のタグを追加する   

```
<meta name="viewport" content="width=device-width, initial-scale=1">
```

- `width=device-width`は`viewport`の幅をデバイスの幅に合わせてくれます
- `initial-scale=1`はページのロード時のズームを一倍に設定します。
- `user-scalable=no`で、ズームができなくするようにもできます。が、モバイル端末の向きを変えたりして、ズームが変わった時に、ユーザーが元のズームに戻す方法がなくなるので、個人的には設定しない方が良いかなと感じています。

<!--more-->

## iOS Safariでの動作確認とデバッグ
simulatorと実機を使った方法の2つがある。

### 1. simulator

1. Xcodeを開いて、メニューのXcode > Open Developer Tool > iOS Simulatorをクリック
2. iOS Simulator上のSafariで動作確認したいサイトを開く
3. Mac側でもSafariを開く
4. Mac側のSafariのメニューのDevelop > iOS Simulator > Safariで開いているサイトの一覧が表示されるので、動作確認したいサイトを開く
5. inspectorが起動するので、あとはデスクトップ用のウェブサイトの時と同じように

### 2. 実機

1. Macにiphoneをつなぐ
2. iphoneのSafariで動作確認したいサイトを開く
3. Mac側のSafariのメニューのDevelop > iOS Simulator > Safariで開いているサイトの一覧が表示されるので、動作確認したいサイトを開く
4. inspectorが起動するので、あとはデスクトップ用のウェブサイトの時と同じように


## Andriod Chromeのデバッグ方法
### 実機
1. Android端末のSettings > Developer optionsでUSB debuggingにチェックをつける
2. Macのchromeのアドレスバーに`chrome://inspect`と入力。Discover USB Devicesにチェックをつける
3. AndroidをUSBでMacに繋ぐ
4. Macのchromeで先ほど開いた`chrome://inspect`に、端末とその端末のchromeで開いているタブの一覧が表示される。
5. 動作確認したいサイトの下にあるinspectをクリックすると、inspectorが軌道する

## Tips
### 1. タップ時にelementがグレーにハイライトされるのを防ぐ

```
-webkit-tap-highlight-color: rgba(0, 0, 0, 0)
```

### 2. リンク長押し時のポップアップメニュー表示を防ぐ

```
-webkit-touch-callout: none;
```

### 3. スクロールバーを非表示にする

```
::-webkit-scrollbar {
  display: none;
}
```

### 4. media query in js

```
// returns boolean
window.matchMedia('(max-device-width : 480px)').matches
```

### 5. Detecting touch devices

```
'ontouchstart' of window or (navigator.MaxTouchPoints > 0) or (navigator.msMaxTouchPoints > 0)
```


## はまった点
- Android Chromeでは、`keypress`イベントが呼ばれない
- Android Chromeでは、`event.keyCode`または`event.which`が常に`0`である
- Mobile SafariもAndroid Chromeも`input.value = newValue`をすると`input`イベントが二回呼ばれる。
- 環境によって呼ばれる[イベントの順番が違う](http://patrickhlauke.github.io/touch/tests/results/)
- touchendとmousemoveの反応領域が違う

## 終わりに
毎週更新を心がけていたのですが、間が空いてしまった。色々と`objective-c`に関する記事を書き溜めていたのですが、`Swift`の登場で、記事自体の価値が低下してしまって、どうしようかと考えています。`Swift`に関する記事を、自分の勉強のまとめも兼ねて投稿していこうかなと考えています。

## cf
https://developer.chrome.com/devtools/docs/remote-debugging

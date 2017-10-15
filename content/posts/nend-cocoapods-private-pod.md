---
title: "nend SDKをCocoaPodsのPrivate Podsとして管理する"
date: 2014-06-22 00:57:48 -0700
tags: [iOS, CocoaPods, nend]
---

iphoneアプリに表示する広告として、[nend](http://nend.net/)を使っているのですが、いかんせんCocoaPodsに対応していないため、

1. 新しいバージョンが出た時
2. 新しいプロジェクトに追加する時
3. 新しいマシンにした時

に毎回手動でライブラリをダウンロードやら、追加しないといけません。「3回繰り返したら自動化」に倣い、自動化する事にしました。

<!--more-->

方法としては、nendをprivate podとして作成します。

## Step 1: nendのSDKをダウンロードする
ログインして、SDKをダウンロードします。「広告枠の管理」 > 「サイト/アプリ」 > 「広告枠」> 「SDK」からダウンロードできます。

## Step 2: NendAd directoryの中身を取りす
ダウンロードしたSDKは以下のディレクトリ構造になっています。

```
.
├── AdMobMediationAdapter
└── Nend
    ├── NendAd
    │   ├── NADIconArrayView.h
    │   ├── NADIconLoader.h
    │   ├── NADIconView.h
    │   ├── NADView.h
    │   ├── NADView_readme.txt
    │   └── libNendAd.a
    ├── Samples
    └── nendSDKiOS2.4.0_manual.pdf
```

必要なファイルは大体`NendAd`以下にありますね。CocoaPodsでは`Classes`ディレクトリ以下にsource fileを置く事を推奨しているので、ファイルをコピーして以下の様なディレクトリ構造にします。

```
Nend
├── Classes
│   ├── NADIconArrayView.h
│   ├── NADIconLoader.h
│   ├── NADIconView.h
│   ├── NADView.h
│   └── libNendAd.a
└── NADView_readme.txt
```

## Step 3: Nendをgitで管理
Step 2で作成したNendディレクトリに移動し、git repositoryを作成します

```sh
$ git init
```

またgithub/bitbucketにてremote repositoryを作成し、remote repositoryとして追加します。

## Step 4: podspecの作成
`pod spec create`を使って、podspecを作成します。ここでは、名前を`Nend`にします。

```
pod spec create Nend
```

## Step 5: pod specに記入
podspecの中身を埋めていきます。バージョンはnendライブラリのバージョンに合わせます。

```
Pod::Spec.new do |s|
  s.name         = "Nend"
  s.version      = "2.4.0"
  s.summary      = "Private pod of Nend Ad"
  s.homepage     = "https://www.nend.net/m/"

  s.description  = "Private pod of Nend"
  s.license      = "MIT"

  s.author       = "Nend"

  s.platform     = :ios
  s.ios.deployment_target = "6.0"

  # REMOTE_REPO_URLの部分は、Step 3で追加したリモートレポジトリのURLを追加します
  s.source       = { :git => "REMOTE_REPO_URL", :tag => "2.4.0" }
  s.source_files  = "Classes", "Classes/*.{h,m}"

  s.frameworks = "AdSupport", "Security"
  s.vendored_library   = 'Classes/libNendAd.a'

  s.requires_arc = true

end
```

## Step 6: remote repositoryにpush

1. コミットをします(`git commit -am 'Add Nend Library 2.4.0'`)
2. バージョンと同じ名前のタグを付けます(`git tag -a 2.4.0 -m '2.4.0'`)
3. リモートにpushします(`git push origin -u`)
4. tagもpush(`git push origin 2.4.0`)

## Step 7: プライベートなSpecレポジトリを作成
[Cocoapods本家のSpecレポジトリ](https://github.com/CocoaPods/Specs)に類似したディレクトリ構造を持つレポジトリを作成します。

```
mkdir Spec; cd Spec
git init
```

また、bitbucket/githubにremote repositoryを作成し、remoteとして追加します。

## Step 8: プライベートなSpecレポジトリをCocoaPodsに追加

```
# Step 7でリモートとして追加したレポジトリのURLをREMOTE_REPO_URLの部分に書きます。
pod repo add Spec REMOTE_REPO_URL
```

追加されたかどうか確認するには、以下のコマンドを走らせます。

```
pod repo lint ~/.cocoapods/repos/Spec
```

## Step8: Nend.podspecをSpecに追加

```
pod repo push Spec Nend.podspec --only-errors
```

動作確認として、Nendディレクトリに移動し、pod spec lintを走らせてみましょう。

```
pod spec lint Nend.podspec --allow-warnings
```

これで終わりです。

## cf
http://guides.cocoapods.org/making/private-cocoapods.html

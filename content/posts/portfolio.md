---
layout: post
title: "middlemanで作ったportfolioをS3で運用"
date: 2014-11-22 01:22:05 -0800
comments: true
tags: [middleman, s3, namecheap, portfolio]
---

[portfolio](http://www.parakeety.com/)を作成しました。本当は他にも作ったものがあるのですが、デザイン等色々修正する必要があるので、追々追加して行こうと考えています。

<!--more-->

## 構成
### [Middleman](http://middlemanapp.com/)
MiddlemanはJekyllのような静的サイトジェネレーターです。使ってみた感想としては、シンプルな構成で自分好みでした。slimとsassを使える上、livereloadのおかけで、開発がスムーズに進みました。

### Amazon S3
ホスティングはAmazon S3のStatic Website Hostingという機能を使っています。namecheapでDomainを購入したのですが、Route53のDNSサーバに変更するのに少し手間取りました...

### [middleman-sync](https://github.com/karlfreeman/middleman-sync)
毎回ビルドしたhtmlやらcssを手動でAmazon S3にアップロードするのは少し面倒なので、[middleman-sync](https://github.com/karlfreeman/middleman-sync)というgemを利用しました。middleman-syncを利用する際にAWSの`access key id`と`secret access key`を渡す必要があるのですが、`config.rb`に直に書くのは避けるために、環境変数を参照するように設定しました。

```ruby
sync.aws_access_key_id = ENV['AWS_ACCESS_KEY_ID']
sync.aws_secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']
```

そこで便利だったのが、[direnv](https://github.com/zimbatm/direnv)というツールで、指定したディレクトリに移動した時のみに指定の環境変数を有効化してくれるツールです。`.envrc`に以下の様に書くと、あとはprojectのルートディレクトリに移動すると`.envrc`に記述した環境変数を読み込んでくれます。

```sh
# .envrc
export AWS_ACCESS_KEY_ID="YOUR_AWS_ACCESS_KEY_ID"
export AWS_SECRET_ACCESS_KEY="YOUR_AWS_SECRET_ACCESS_KEY"
```


## おわりに
とてもmiddlemanを気に入ったので、時間を見つけて本ブログもoctopressからの移行を画策しています。

## cf
- http://blog.qnyp.com/2013/05/21/middleman-sync
- https://www.youtube.com/watch?v=KTFWnnVi1oA
- https://github.com/zimbatm/direnv
- http://dev.classmethod.jp/tool/direnv

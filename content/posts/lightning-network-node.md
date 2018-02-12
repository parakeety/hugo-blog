---
title: "Lightning Networkのノードを立てた"
date: 2018-02-12T09:50:00+09:00
---

Lightning Networkのノードをmainnetに立てました。基本的には[Run your own mainnet Lightning Node](https://medium.com/@dougvk/run-your-own-mainnet-lightning-node-2d2eab628a8b)に従ってやりました。

<!--more-->

## 大まかな流れ

1. Bitcoinのfull node(bitcoind)を立て、最新のblockまで同期する
2. Lightningのnode(lightnind)を立てる
3. Lightningのnodeで、新しくBitcoinのアドレスを作成し、そのアドレスにBitcoinを入金する
4. 他のLightningのnodeに接続する
5. 4.で接続したchannelに入金する

です。

1.の同期と、Bitcoinの入金に結構時間がかかります。
あと記事内で使われている `lightning-cli getpeers` は、 `lightning-cli listpeers` に名前が変わった様です。
今後も名前が変わる可能性は十分にあるので、 `lightning-cli help` で確認するのが良さそうです。

Lightning Networkの主な実装には、

1. [lnd](https://github.com/lightningnetwork/lnd): Lightning Labsが主導で開発。Goで書かれている
2. [eclair](https://github.com/ACINQ/eclair): ACINQが主導で開発。Scalaで書かれている
3. [c-lightning](https://github.com/ElementsProject/lightning): Blockstreamが主導で開発。Cで書かれている

があり、今回はc-lightningを使いましたが、開発自体はlndが一番活発な様です。次はlndを立ててみようと思います。

実際にLightning Networkを使って、Lightning支払いを受け付けている[Blockstream shop](https://store.blockstream.com/shop/)でstickerを買ってみたのですが、本当に支払いが一瞬で終わり、感動を覚えました。

ACINQ社が、testnetで動く[androidアプリ](https://play.google.com/store/apps/details?id=fr.acinq.eclair.wallet)を公開していて、実際にモバイルから支払いを試すことができます。Lightningのmobile walletの需要は大きそうなので、iOS向けのライブラリやwalletアプリ開発を時間見つけてやっていきたい。


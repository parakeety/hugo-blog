---
title: "Raspberry Pi Bitcoin Full Node"
date: 2018-02-09T10:33:45+09:00
tags:
  - Raspberry Pi
  - Bitcoin
  - Cryptocurrency
---

Raspberry PiにBitcoinのフルノードを立てました。

<!--more-->

## Raspberry Piのスペック
- Raspberry Pi 3 Model B
- memory: 1GB
- OS: raspbian

Bitcoinのフルノードは、データサイズが200GB近くあるので、外付けのハードディスク(2TB)を買いました。

## 手順
1. Raspberry Pi setup(OSインストールから、SSHのsetupまで)
2. 外付けのハードディスクのmount
3. swapの設定
4. Dockerのインストール
5. Docker container起動

## 1. Raspberry Pi setup
まずRaspberry Piのsetupをします。

- raspbianをインストール
- pi userのパスワード変更
- 新規userを作成し、piを削除
- `ssh-copy-id`を使って公開鍵をRaspberry Piにコピー
- sshのportを変えたり、passwordログインやrootでのログインを禁止する

私はmoshを使っているので、moshもRaspberry Piにインストールしました。

## 2. 外付けのハードディスクのmount
[この記事](http://sooch.hatenablog.com/entry/2017/03/26/061032)の様に、ext4にformatして、mount

## 3. swapの設定
Raspberry Piはメモリが1GBしかないため、bitcoindを走らせるためにはメモリが結構ギリギリです。swapの設定を何もしなかったら、`Out of memory`でbitcoindがエラーで終了しました。

```sh
# /etc/dphys-swapfile
CONF_SWAPSIZE=1000 #100から1000に変更
```

```
$ dphys-swapfile setup
$ dphys-swapfile swapon
```

## 4. Dockerのインストール
```
$ curl -sSL https://get.docker.com | sh
```

を実行して、Dockerをインストールします。

## 5. Docker container起動
Raspberry PiでDockerを扱う場合に一つ注意が必要なのは、Raspberry PiのCPUはARMなので、x86向けにbuildされたDocker imageが使えません。
なので多くのDocker imageがそのままでは使えないので、自分でimageを作る必要があります。
今回は[jamesob/docker-bitcoind](https://github.com/jamesob/docker-bitcoind)をforkして[自分でimage](https://github.com/parakeety/docker-bitcoind)を作るようにしました。
baseイメージをRaspberry Pi向けに`arm32v7/ubuntu`に変更したのと、メモリが足りなくなるのを防ぐために`bitcoin.conf`をいくつか変更しました。

```
blocksonly=1
dbcache=20
maxsigcachesize=4
maxconnections=4
rpcthreads=1
```

あとはdocker runするだけです。YOUR_USERNAMEとYOUR_PASSWORDとVOLUME_PATHは適宜変更して下さい。
```
docker run --name bitcoind -d \
--env 'BTC_RPCUSER=YOUR_USERNAME' \
--env 'BTC_RPCPASSWORD=YOUR_PASSWORD' \
--volume VOLUME_PATH:/bitcoin \
-p 8332:8332 \
-p 8333:8333 \
parakeety/bitcoind-test
```

## 同期速度
bitcoin full nodeはblockの同期時に、署名の検証もしている様で、マシンパワーが同期速度にかなり影響します。Digital Oceanで借りたインスタンス(6vCPU, memory 16GB)だと12時間で同期が終わったのに対し、Raspberry Piは3日経った今でも最新のblockまで同期が完了していません。急ぎの場合は、Raspberry Piではなく、もっとスペックの良いマシンでfull nodeを立てた方が良さそうです。
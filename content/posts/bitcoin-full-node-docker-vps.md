---
title: "BitcoinのフルノードをVPS上のDockerで運用開始"
date: 2017-11-17T19:40:20+09:00
---

勉強も兼ねて、Bitcoin Core(いわゆるfull node)をVPS上のDockerで運用し始めました。

<!--more-->

## 構成
- さくらVPS
- HDD: 200GB(少なくとも145GB必要 *2017/11/17時点) 
- CentOS 7
- Docker

Docker imageは、`bitcoin.conf`という設定ファイルに`txindex=1`を追加したかったので、[jamesob/docker-bicoind](https://github.com/jamesob/docker-bitcoind)を元に自前の[Dockerfile](https://github.com/parakeety/docker-bitcoind)を作成して使っています。
Bitcoin CoreにはRPCサーバーがついていて、JSON-RPCでBitcoin Coreと通信ができるのですが、`bitcoin.conf`にRPCのusernameとpasswordを指定し、`docker run`する時に指定する必要があります。

```
# bitcoin.conf
txindex=1
rpcuser=username
rpcpassword=password
rpcport=8332
```

```
docker run --name bitcoind -d \
    --env 'BTC_RPCUSER=username' \
    --env 'BTC_RPCPASSWORD=password' \
     -v data_volume:/bitcoin \
     -p 8333:8333 \
     -p 127.0.0.1:8332:8332 \
     parakeety/bitcoind-test
```

## 運用を始めて
ブロックの同期にかなり時間がかかっています。bitcoindを走らせてから4日ほど経っていますが、Blockの同期がまだ終わっていません。Block heightが426524まで来ているので、あと少しで最新のBlockまで追いつきそうです。
フルノードを運用するインセンティブとしては、

1. networkへの貢献
2. privacy

くらいでしょうか。Bitcoin-cliを勉強することが目的なら、testnetで十分な気がします。今回運用を始めたのは、運用経験を積んでおきたかったからというのが大きいです。
Bitcoin-cliを触りつつ、その内Raspberry PiとGKEに移す事を考えています。

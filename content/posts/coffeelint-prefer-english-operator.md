---
title: "coffeelintのcustom ruleを作った"
date: 2014-05-31 22:28:05 -0700
tags: [coffeescript, coffeelint, custom rule, npm]
---

tl;dr: `==`、`!=`、`&&`、`||`を使っている時に、warningを出す`coffeelint`の[custom rule](http://www.coffeelint.org/)を作ったよ

<!--more-->

coffeescriptの有名なcoding styleとして、[polarmobile](https://github.com/polarmobile/coffeescript-style-guide)があります。このcoding styleに則って書いているのですが、coding styleを維持するために、何か良いツールがないか探した所、[coffeelint](http://www.coffeelint.org/)なるものを見つけました。

`coffeelint`でもある程度カバーできるのですが、カバーできない部分で、自分で実装できそうな部分を[custom rule](https://github.com/parakeety/coffeelint-prefer-english-operator)として作りました。

具体的には、`==`、`!=`、`&&`、`||`を使っている場合に、warningを出すルールです。polar mobileのcoding styleでは

```coffeescript
i is i # Yes
i == i # No
```

の様に、英語風のoperatorを優先して使う様に、勧めています。`coffeelint`標準のオプションでは、なかったので、実装しました。`coffeelint`のcustom ruleを作る事自体は簡単で、

```coffeescript
module.exports = class RuleProcessor
  rule:
    name: 'custome_rule_name'
    description: 'description of custom rule.'
    level: 'error' # or 'warn'
    message: 'message shown when lint failed'
  # add one from below
  lintLine: (line, lineApi) ->
    # return true if lint fails
  lintToken(token, tokenApi) ->
  lintNode(node, nodeApi) ->
```

で終わりです。あとは`lintLine`、`lintToken`、`lintNode`の中身を実装するだけです。今回作ったcustom ruleは、正規表現を使った簡単なもので、

```coffeescript
lintLine: (line, lineApi) ->
  /[&|\||\=]{2}|\!\=/.test(line)
```

のみです。

使う時は、

1. `npm install`で`coffeelint-prefer-english-operator`をインストール
2. `coffeelint --makeconfig > coffeelint.json`で`coffeelint.json`を作成
3. `coffeelint.json`に以下を足します

```json
"prefer_english_operator": {
 "module": "coffeelint-prefer-english-operator",
 "level": "error"
}
```

あとは`coffeelint *.coffee`でlintしたい`coffee`ファイルを指定すれば、lintしてくれます。


## 感想
`npm`に不慣れで、色々とハマりましたが、custom ruleを作る事自体はとても簡単でした。

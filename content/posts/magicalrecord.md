---
layout: post
title: "Core Dataで書かれたコードをMagicalRecordで置き換えた"
date: 2014-12-17 01:29:52 -0800
comments: true
tags: [iOS, MagicalRecord, 'magical record']
---

昔作ったアプリを今修正しているのですが、Core Data周りのコードが書くのが辛かったので、[MagicalRecord](https://github.com/magicalpanda/MagicalRecord)に移行しました。

<!--more-->

## MagicalRecordとは
Core Dataのラッパーです。裏側はCore Dataなので、Core Dataの高パフォーマンスの恩恵を受ける事ができます。

## Core DataとMagicalRecordの比較
```objective-c
// core data
NSManagedObject *adam = [NSEntityDescription insertNewObjectForEntityForName:@"Employee"
                                                      inManagedObjectContext:context];

// MagicalRecord
Employee *adam = [Employee MR_createEntity];
```

オブジェクト指向に慣れている方や、RailsのActive Recordに馴染み深い方は、MagicalRecordの方がすっきり見えるのではないでしょうか。私はEntity名をStringで渡すのが辛くて、`NSManagedObject`のサブクラスにMagicalRecordの様なクラスメソッドを自前で実装していたのですが、今思うと最初からMagicalRecordを使っていればよかった...

## 移行方法
### 1. App Delegateに生成されるCore Data関連のコードを削除
プロジェクトを作るときに、'Use Core Data'をチェックすると、App DelegateにCore Data関連のコードが生成されます。このCore Data関連のコードをごっそり削除します。

### 2. データベースのファイル名を渡す
MagicalRecordにはいくつかsetupに関するメソッドがあるのですが、その内の一つに、データベースのファイル名を渡します。
```objective-c
[MagicalRecord setupCoreDataStackWithStoreNamed:@"DATABASE_NAME.sqlite"];
```

これで移行は完了です。


## 終わりに
Core Dataはかなり学習コストが高いと個人的には思うのですが、MagicalRecordなら気軽に始められるのではないでしょうか。今回は既にCore Dataを導入しているアプリだったので、Magical Recordを使いましたが、Mobile時代の新しいデータベースである[Realm](https://github.com/realm/realm-cocoa)も気になっているので、新しいアプリを作るときに試してみたいなと思っています。

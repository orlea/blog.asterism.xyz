+++
author = "aries"
categories = ["diary"]
tags = ["obsidian", "VPS"]
date = 2023-12-04
title = "Obsidian始めました"
# description = "" # 検索エンジン用
featured = false
featureImage = "/img/2023/obsidian-workspace-202312.png" # 記事トップ画像
featureImageAlt = "Obsidianワークスペース"
thumbnail = "/img/general/obsidian-icon.png" # 記事リストから見えるサムネ
type = "post"
url = "posts/9ba872b5a6e1f2f28325fa7b04192c55"
toc = false
draft = false
+++

内容は主に生活系のメモとゲーム関連あとは技術関連(Mastodonサーバーの運用だとか)、ブログの下書きだとかの予定。

この手のツールはMS Loopしか使ったこと無くて良い印象無かったけどObsidianは結構気に入った。

こうなるとNotionも気になるね。


## 経緯
- 元々はgoogle workspaceを契約しているのもあって、google keepを使っていた
- 簡単に使えて特に何も考えずとも勝手にいい具合に同期してくれて、超手軽
- ただ手軽が故に機能は少ない
- 整理できなくなってきた
- keepは付箋みたいなもので、後から見返すのは結構しんどい
- 最近見返すメモがまぁまぁ多いなーとなり他ツールも使ってみるのを決意

## 作った環境
- Self-hosted LiveSyncを使ってPCとipad miniの同期環境を作った
- DBサーバーはおもちゃにしてるWebArena Indigoにdocker-composeでさくっと構築
- タグやフォルダは一旦適当に
	- Obsidian Tag Wrangler Pluginのお陰で後から整理しやすい
	- ので適当にざくざく作れるように
	- keep的な使い方もできるようにobsidian-memosも入れてみたけど、実際どうかはしばらく使ってみなきゃわからない？
- スマホとiPad miniも同期取る所までは設定済み
    - 通信量がちょっと心配？(結構スクショとかもvaultに突っ込むと思うので)

## PCに入れたプラグイン
- 2Hop Links plus
	- いらんかも
- Advanced Tables
- Obsidian Memos
- Recent Files
- Self-hosted LiveSync
- Settings Search
	- 設定項目多くて探すのしんどいのでめっさ便利
- Style Settings
	- とりあえず入れてみたけど要らないかも
- Tag Wrangler
	- 超便利
- Tag Folder
	- あんまいらんかも
- Custom Frames
	- Obsidianの中でgoogle keepとかgoogle calenderとか開ける
- Editing toolbar
	- MDの書式あんまり覚えてないから有ると助かる


## その他設定

フォントがめっさ中華だったので適当にSourceHanCodeJP設定したりしました。

## おまけ

Self-hosted Livesync用DBサーバー(couchdb)を立てるdocker-compose.yml



```docker-compose.yml
version: '3'

services:
  couchdb:
    image: couchdb:latest
    ports:
      - 5984:5984
    environment:
      - COUCHDB_USER=USERNAME
      - COUCHDB_PASSWORD=PASSWORD
    volumes:
      - ./dbdata:/opt/couchdb/data
```

TLS対応しといた方が良いんじゃろうが(ちょっと触ればすぐできるけど)「やっぱやーめた」ってなったとき要らないメール増えるからそこまではやってない。

公式ドキュメントだとどっかのクラウドサービスの無料枠でやるのが楽やで🤘って書いてたけど、これだけのためにクレカ登録までやんのもな……となり普段遊んでるVPSに追加。

ちなみに今回も前にブログで書いたWebArena Indigoで動いてます。(これもやめて他のサービスに移したい。安いとは言えしょぼすぎるんだよなーーー)
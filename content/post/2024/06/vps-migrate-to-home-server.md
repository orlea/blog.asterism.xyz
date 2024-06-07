+++
author = "aries"
categories = ["tech"]
tags = ["navidrome", "サーバー", "docker"]
date = 2024-06-07
title = "趣味用VPSを自宅サーバーに移しました"
# description = "" # 検索エンジン用
featured = false
# featureImage = "/img/main/logo.png" # 記事トップ画像
# featureImageAlt = "ロゴ画像"
# thumbnail = "/img/main/logo.png" # 記事リストから見えるサムネ
type = "post"
url = "posts/382bed08a5818969a022e919e518cba1"
toc = false
draft = true
+++

# 経緯

1年ほど前にこのブログで、円安が理由で趣味用VPSをLinodeからWebarena Indigoに移したという記事を投稿しました。

乗っけていたものはざっくり以下
- 音楽用navidrome(subsonic互換サーバー)
- 音声作品用navidrome
- google driveからメディアファイルをマウントするrclone
- n8n(discord botとかnavidromeからmastodonへのなうぷれ用webhookとか)
- リバースプロキシ用caddy
- obsidian同期用couchdb

日常的に使ってはいましたが
- いかんせん最安インスタンスのためスペックがしょぼい
- rcloneでgoogle driveをマウントしてる都合上navidromeが遅くてストレス
- そのストレスの結果あまりnavidromeを使わない日々が続きVPS代金がもったいなく感じる(月500円くらい)

といった具合で、なんとも安物買いの銭失い的な感じになっておりました。

そんな中久しぶりにスマホとipad miniのobsidianを同期しようとした所エラーで失敗。

中々修正に手間取り一旦直したもののあまり使い続ける気にならず同期についてはRemotely-SaveとS3を使う形に変更しました。(こっちはこっちでそのうち何か書くかも)

そもそもnavidromeに食わせるメディアファイルは家のファイルサーバーにも置いてあるため、オンプレに置いた方が高速な動作が期待できる。(navidrome立てた当初は家には無かった)

VPSを維持する理由が1つ無くなり、また自宅サーバーのリソースが余り気味で活用したく、かねてより試したかったcloudflare tunnelを使うチャンスでもあると思い移行を決定しました。

# 移行と現在

元々全部docker composeで動かしていたので移行はかなり単純でした。

ざっくりの作業内容は以下の通り
- 移行先の自宅Ubuntu VMの準備(ストレージ32GB/vCPU4コア/RAM4GB)
- 元のVPSでコンテナを全て停止
- ディレクトリごとアーカイブしてsftpで輸送と解凍
- docker volumeプラグインでrcloneを使っていた所を、家のファイルサーバーをSMB(CIFS)でマウントするように変更
- リバースプロキシの利用を止めて、直接コンテナのポートをホストのポートにバインドするように変更
- cloudflaredのインストールと設定

サンプルとしてdocker-compose.ymlも置いときます。

こんなのを書いて、docker-compose up -dして、cloudflare tunnelの設定をすればいい感じに家へアクセスできて非常によろしいです。通勤中でもオタク音源聴き放題。

NASとか持ってる方なら例えばNASにライブラリ置いておいて、昨今流行りのミニPCにUbuntu入れて、とかやると似たような事をいい具合にできそう。

```
services:
  voice:
    image: deluan/navidrome:0.52.5
    user: 1000:1000
    ports:
      - 4534:4533
    volumes:
      - voice:/music:ro
      - ./nd_voice_data:/data
    environment:
      - ND_IMAGECACHESIZE=1GB
      - ND_SEARCHFULLSTRING=true
      - ND_SCANSCHEDULE=0
      - ND_SESSIONTIMEOUT=8760h
      - ND_DEVENABLESHARE=true
    restart: always
  music:
    image: deluan/navidrome:0.52.5
    user: 1000:1000
    ports:
      - 4533:4533
    volumes:
      - music:/music:ro
      - ./nd_music_data:/data
    environment:
      - ND_IMAGECACHESIZE=1GB
      - ND_SEARCHFULLSTRING=true
      - ND_SCANSCHEDULE=0
      - ND_SESSIONTIMEOUT=8760h
      - ND_DEVENABLESHARE=true
    restart: always

volumes:
  music:
    driver_opts:
      type: cifs
      o: "user=username,domain=fileservername,password=password"
      device: "//192.168.1.1/music"
  voice:
    driver_opts:
      type: cifs
      o: "user=username,domain=fileservername,password=password"
      device: "//192.168.1.1/voice"
```

cloudflare tunnelを使う以上当たり前ですが、navidromeのログイン画面やsubsonic apiのアクセスポイントがインターネットにさらされることになるのでパスワードには注意しましょう。

移行前はかなりトランスコードの待ち時間があってどんどん使う気が失せてしまうような具合でしたが、今はかなり軽快に動いてくれています。ファイルが大きい音声作品でもすぐ再生される事がここまでありがたいとは…。(まぁそもそもdlsite playで良くねって話ではあるんですが)

ちなみにクライアントはPCはブラウザから直接、スマホはSymfonium(Android/有料)とSubStreamer(iOS/無料)を使っています。iOSは使用頻度低め。
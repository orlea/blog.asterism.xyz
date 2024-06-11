+++
author = "aries"
categories = ["tech"]
tags = ["hugo", "SSG", "amplify"]
date = "2023-09-12"
title = "このブログのテーマをhugo-clarityに変更しました"
featured = false
featureImage = "/screenshot20230911.png" # 記事トップ画像
featureImageAlt = "新サイトスクリーンショット"
thumbnail = "/screenshot20230911.png" # 記事リストから見えるサムネ
type = "post"
url = "posts/c05acf7b626a289355b675c1b6fd7ce3"
toc = true
usePageBundles = true
+++

結局このブログのテーマを変更して対応しましたとさ。

## 経緯

[前回の記事：Hugo本体のアプデに追従できないって話](https://blog.asterism.xyz/posts/2023-07-24/)

- 私はweb周りに疎いのでhugoのテーマは他人産のを使いたい
- でもHugo本体のアプデ方針が ~~バカ~~ 前衛的でテーマのメンテナンスが大変で、メンテナンスされなくなったテーマがいっぱいある
- 私が使っていたテーマhugo-imperfect-slimもそうなってしまった
- 今後どうしよう？

あれこれ考えたり試したり(それこそレンサバ借りて久々にWordPress試したりもしてました)した結果、「まぁちょっとくらいがんばるかぁ……」という思いが出てきた。

が、いつまでやるかは分からんね……。

## あれこれ試してる間にやったこと

### WordPress編 2023年7月末~8月半ば

- 引き継ぎたい記事のURLリストを作る
  - Google Analyticsから見てアクセス数が多いページをリストアップ
- さくらのレンタルサーバーで安いプラン借りてWordPressを久しぶりに試す
  - テーマはcocoon(10年くらい前学生の頃WPでブログ書いてた頃、同じ作者さんのsimplicity2を使ってた為)
- WordPressへの移行を考えた時の気になる点を一通り試す
  - ログインページの隠蔽やMFA(All in one securityプラグインを利用)
  - パーマリンク変更に伴うリダイレクト検証(Redirectionというプラグインでお試し)
  - CloudFront通せるかどうか確認(できるけど超簡単とはいかなそうだったので断念)
  - 管理画面と公開画面で別ドメインにできないかあーだこーだ試す(結局諦め)
  - 静的サイト化プラグインも調べましたが、どれもドキュメント不足あるいは積極的にメンテナンスされていないため断念
  - ざっくりページデザイン作ってみる
- 残りはDNSと記事を移せばサイト移行ほぼ完了できるなーって所まではやった

WordPress久々に触って再実感したけど、やっぱ便利ではあるんだよなーレンタルサーバーで適当にサイト立てるくらいなのなら。

ただ何でもかんでもプラグインなせいで設定がだるいってのとPHPなせいでレンサバ以外だとデプロイと運用がまぁまぁ面倒。

アプデちゃんとし続けるってのも必要だし、しんどいかなとなって中止した。

結局書く気力があるタイミングじゃないと触らないから、放置されてる時間が長くなってしまい。それなら静的サイトのがまだ良いんじゃない？という感じ。

逆に言えば強制的に管理画面開く事になるんだから、記事書く頻度も上がるんじゃない？というのはある。

CDNの設定もっと超簡単だったり、公開ページと管理ページのドメイン簡単に変えられたりすれば良いんじゃが。(多分マルチサイト化すればいいんだろうけど…)

docker-composeで適当にどーんとできると楽だけど、テーマとかプラグインによっては結局なーっていう。(DBのバックアップだけではなく、ドキュメントルート配下のphpファイルとかも全部バックアップ取りたいから、そうなるとdocker使うことでの利点がphpfpmの導入楽ってくらい)

### Hugo編 2023年8月末

- 元々使ってたテーマのフォークを探してなんとか最新版Hugoでビルドできるように
  - ```サイトルート/layouts/partials/``` を使って上書き
  - しばらく大丈夫そうになったので精神的に少し落ち着く
- 上手いこと移行できそうでメンテナンスされてそうなテーマを探す。見つかった候補は以下
  - Mainroad
    - トップページに過去記事がリストアップされなかった
    - mainSectionsの設定を変えればできそうなんだけど、よく分からず混乱しているうちにやる気を無くす
  - Stack
    - Mainroadを諦めた数日後試した。割と良い感じだった
    - けど、aboutページなんかが上手く行かなかず結構時間かかって疲れる
    - 結局これはabout/_index.mdをabout/index.mdに変えたら直った
    - 疲れた後、右サイドバー上手く行かねーとなり諦め(stashは残してある)
  - Hugo Clarity
    - stackを試した直後に見つけて色々試してたらなんとかなった(ほんとか？)
    - 結構積極的にメンテナンスされていそう
    - なにより中華フォントが設定されていないから日本語がキモく見えない
    - デフォで中華フォントが埋め込まれてるテーマだとcssなりを自分で上書きする必要があり、フロントエンド弱者にはしんどい

## 最終的にやったこと

- サブモジュールとしてHugo Clarityを追加
- config.tomlを削除
  - いつの間にかconfig.tomlじゃなくてhugo.tomlがスタンダードになったっぽい
- hugo.tomlを書く
  - ```hugo server``` でチェックしながら設定をいじっていく
- 過去記事を ```content/posts``` から ```content/post``` に移動
  - 他のテーマで上手く行かなかったビルドもこれやればなんとかなったかも？
  - あるいはmainSectionsいじればよかった？(少し試したけど上手く行かず)
- aboutページのファイル名を _index.mdからindex.mdに変更
- ```layouts/partials``` にテーマ自身の ```layouts/partials/head.html``` をコピー
  - Mastodonの自サイト判定がちゃんと行われるように、こんなのを最下部に追加
  - ```<a hidden rel="me" href="https://mstdn.asterism.xyz/@aries">Mastodon</a>```
- ```i18n/ja.toml``` にテーマ自体の ```i18n/en.toml``` をコピー
  - hugo.tomlではlanguageをjaにしたいけど、見た目まで英語にする気はなかったから
  - サイト設定側のlanguage未設定なら良いんじゃない？→htmlのlangがenになっちゃう
- ```assets/sass/_override.sass``` で色をいじる
- ```assets/sass/_custom.sass``` でscroll-behaviorをautoにした
  - デフォのsmoothだとなんか画面更新した時に勝手に妙なスクロールされる
  - 理由良く分からず。chrome, vivaldiで確認(プラグイン無しでも起きた)
- ```static/main/``` に前とは違うアイコンを置いた
- ```static/icons``` 以下にファビコンを配置変え
- フロントマターでのlastmod(記事更新日)をサポートするようにテーマを修正
  - これも ```layouts/partials/post-meta.html``` にテーマ自体のファイルをコピーしてきて上書きさせてる
  - あると嬉しいなーって機能だったためPR送ってみたけど、ちょっと見た目がしんどい感じ
  - 実験としてこんなのも作ってみたが、まー微妙かね
  - https://mstdn.asterism.xyz/@aries/111047799743963059
- 今後の記事のURLをランダムにするため、archetypes/post.mdをいじった
  - 後から編集することもあるしURLが日付なのもあんまりなぁと思った
  - ただの日記なら良いだろうけど、技術的な内容が入ると更新することも偶にある
  - だからといって毎度毎度URL考えるのは面倒だからmd5 hashを使うように変えた
  - 過去記事はそのまま。そのままにするために過去記事全部のフロントマターを編集した(40ファイルくらい)
- AWS Amplifyのビルドスクリプト(amplify.yml)を編集した
  - アイコンやら画像類のパスがぶっ壊れたため

``` yaml
version: 1
frontend:
  phases:
    build:
      commands:
        - wget https://github.com/gohugoio/hugo/releases/download/v0.117.0/hugo_extended_0.117.0_Linux-64bit.tar.gz
        - tar -xf hugo_extended_0.117.0_Linux-64bit.tar.gz
        - mv hugo /usr/bin/hugo
        - rm -rf hugo_extended_0.117.0_Linux-64bit.tar.gz
        - echo $(hugo version)
        - if [ "${AWS_BRANCH}" = "master" ]; then export BASEURL="https://blog.asterism.xyz/"; fi
        - if [ "${AWS_BRANCH}" != "master" ]; then export BASEURL="https://pr-${AWS_PULL_REQUEST_ID}.${AWS_APP_ID}.amplifyapp.com"; fi
        - hugo -F -b ${BASEURL}
  artifacts:
    baseDirectory: public
    files:
      - '**/*'
  cache:
    paths: []
```

動いてるしmaster以外はPR起こされたらビルドするようになってるから特に問題は怒らないんだけど、でもまぁもうちょいやり方あるでしょって感じ(試すのが面倒で直す気はあまりない)

リポジトリに残ってる履歴はこんな感じ。

https://github.com/orlea/blog.asterism.xyz/commits/master

2023/9/11, 9/12のコミットがそれ。


## 今後

記事を書きたいタイミングは、サイトをいじるって事に対しても結構積極的になれるけどそうじゃない時はあんまり気分よく頑張れないねぇってなっている。

今回はスタートが後ろ向きだったのもあったけど、でもまぁ久しぶりにこんな事して楽しくなかったかというと嘘になる。

スクロールガクつく問題調べたりも、まぁ珍しく自力でなんとかできて嬉しいね。

ちょっと時間かけて頑張ったわけだし、まーなんとか運用していきたいねぇ。

ゲームやら映画やらの感想なんかをもうちょい腰据えて書くのも良いんだけど、現状この辺りってMastodonに書いてるのもあってあまりこっちのサイト更新することもないなーというのが2022年ほぼ更新なかった理由の一つ。

まぁもっと適当に、遊んだゲームの記録を取るくらいの感じで書いてもいいかも。

Mastodonに書き散らしてるのは、ゲームのタグはつけるようにしてるけど自分が何やったかみたいなのをずらっと眺めてニヤニヤする、みたいなことはこのやり方じゃできないし。

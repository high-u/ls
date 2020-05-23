---
title: "ブログはじめる"
date: 2020-05-23T15:07:43+09:00
draft: false
description: "ブログは static site generator の HUGO にした。デプロイ先は Versel。"
tags: ["blog"]
author: ""
---

## HUGO にした

[![HUGO](/images/first-post/hugo.png)](https://gohugo.io/)

### インストール

```bash
brew install hugo
hugo version
hugo new site ls
cd ls

git init

# hello-friend というテーマを導入
git submodule add https://github.com/panr/hugo-theme-hello-friend.git themes/hello-friend
# ここを参照 https://github.com/panr/hugo-theme-hello-friend#how-to-configure
vim config.toml

# 記事作成
hugo new posts/first-post.md
vim content/posts/first-post.md

# サーバー起動
hugo server -D
open http://localhost:1313
```

## デプロイ先は vercel

[![vercel](/images/first-post/vercel.png)](https://vercel.com/)

### vercel の設定

- `Import Project` から github のリポジトリを登録した。
- 設定は下記の様。`FRAMEWORK PRESET` に `hugo` があったが、 `config.yaml` を要求されて、とりあえず諦めた。

![vercel](/images/first-post/vercel-settings3.png)
![vercel](/images/first-post/vercel-settings.png)

- Hook を作成したら Copy する。

![vercel](/images/first-post/vercel-settings2.png)

### github の設定

- Copy した URL を `Payload URL` に貼り付ける。

![github](/images/first-post/github-webhook.png)

## デプロイ

```bash
hugo -D
git add .
git commit -m "xxx"
git push
```

- はじめ、いろいろ試している内に、push してもサイトが更新されなくなった。
- github の Webhooks 見たら、`429` のエラーが出てる。
- 何かと思ったら Vercel の `Deploy hook triggers per hour.` という [limit](https://vercel.com/docs/v2/platform/limits) に引っかかっていた。注意注意。

## 利用中／利用予定のツールたち

### iMage Tools

- サイズ変更、角丸、ドロップシャドーなどの画像加工。

[![iMage Tools](/images/first-post/imagetools.png)](https://apps.apple.com/jp/app/image-tools/id493949693)

### Skitch

- クリップボードから貼り付けてファイル化。

[![Skitch](/images/first-post/skitch.png)](https://apps.apple.com/jp/app/skitch-%E6%92%AE%E3%82%8B-%E6%8F%8F%E3%81%8D%E8%BE%BC%E3%82%80-%E5%85%B1%E6%9C%89%E3%81%99%E3%82%8B/id490505997)

### carbon

- ソースコードを画像化してみたい。リンク先に github かな？

[![vercel](/images/first-post/carbon.png)](https://carbon.now.sh/)

以上

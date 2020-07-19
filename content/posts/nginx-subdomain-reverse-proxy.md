---
title: "Nginx で reverse proxy。 proxy_pass にハマる。"
date: 2020-07-19T13:58:42+09:00
description: "Nginx で、同じ Docker Network 内の、サブドメインと同一のホスト名を持つコンテナに中継する Reverse Proxy を作る。"
tags: ["docker", "nginx", "reverse proxy"]
---

## やりたいこと

- サブドメインと同じホスト名を持つコンテナに中継するリバースプロキシを Docker コンテナで作る。
- 中継先は、同じ Docker Network 内とする。

## 当初は

- [Caddy](https://caddyserver.com/) でやろうと思ったのだが、自分の力では出来そうもなく断念。

## Nginx で出来た

```bash
cat <<EOF > spades.html
<!doctype html>
<html>
<body>
<h1>&spades;</h1>
</body>
</html>
EOF

cat <<EOF > diams.html
<!doctype html>
<html>
<body>
<h1>&diams;</h1>
</body>
</html>
EOF

cat <<EOF > nginx.conf
events {
  worker_connections 1024;
}
http {
  server {
    listen 80;
    server_name "~^(?<subdomain>.+)\.localhost";
    location / {
      resolver 127.0.0.11 valid=5s;
      proxy_pass http://\$subdomain/;
      proxy_redirect off;
    }
  }
}
EOF

cat <<EOF > docker-compose.yaml
version: "3"
services:
  spades:
    image: nginx:1.19-alpine
    volumes:
      - ./spades.html:/usr/share/nginx/html/index.html
  diams:
    image: nginx:1.19-alpine
    volumes:
      - ./diams.html:/usr/share/nginx/html/index.html
  reverse-proxy:
    image: nginx:1.19-alpine
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    ports:
      - 8080:80
EOF

```

- ※ `\$subdomain` の `\` は bash のエスケープ

## ローカルで試す

- hosts ファイルに追記

```bash
sudo vim /etc/hosts
```

- hosts ファイルに追記する内容は下記の通り。

```text
127.0.0.1  spades.localhost
127.0.0.1  diams.localhost
```

- いざ

```bash
docker-compose up
curl http://spades.localhost:8080
curl http://diams.localhost:8080
```

## ハマったのは

- nginx.conf 内の下記の部分。
- [nginx の proxy_pass の闇：株式会社サブスレッド](https://www.subthread.co.jp/blog/20190424/) を参考にさせていただきました。
  - > nginx は起動時に名前解決した時の IP で転送処理を続けてしまいます。
  - > そこで、resolver ディレクティブの valid オプションを使って、TTLのキャッシュ時間を短くしてみました。
- Docker 内蔵 DNS サーバは、 `127.0.0.11` らしいのでそれをセット。
  - [Docker コンテナ・ネットワークの理解 — Docker-docs-ja 17.06 ドキュメント](https://docs.docker.jp/engine/userguide/networking/dockernetworks.html#docker-dns) を参考にさせていただきました。

```text
resolver 127.0.0.11 valid=5s;
proxy_pass http://$subdomain/;
```

---
title: "データベースをデプロイする"
date: 2020-06-13T10:19:16+09:00
description: "最近使っている（遊んでいる）データベースのデプロイ方法のメモ"
tags: ["database", "docker", "kubernetes"]
---

## PostgreSQL

### Docker

- https://hub.docker.com/_/postgres

```bash
docker run -d --rm --name pg -e POSTGRES_PASSWORD=password postgres:12.3

docker exec -it pg psql -U postgres
```

### Docker Compose

```yaml
version: '3.1'
services:
  db:
    image: postgres:12.3
    restart: always
    ports:
      - "5432:5432"
    environment:
      POSTGRES_PASSWORD: password
      PGDATA: /var/lib/postgresql/data/pgdata
    volumes:
      - pg-data:/var/lib/postgresql/data
  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
volumes:
  pg-data:
```

```bash
docker-compose up -d

open http://localhost:8080
#
# データベースの種類: PostgreSQL
# サーバ: db
# ユーザ名: postgres
# パスワード: password
# データベース:
#

docker-compose down
```

### Kubernetes (with kind)

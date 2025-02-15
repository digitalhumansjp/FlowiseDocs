---
description: >-
  Postgresのpgvectorを使用して、エンベッドされたデータをアップサートし、クエリに対して類似性検索を実行します。
---

# Postgres

<figure><img src="../../../.gitbook/assets/image (163).png" alt="" width="292"><figcaption><p>Postgresノード</p></figcaption></figure>

インスタンスの設定方法に基づいて、Postgresに接続する方法は複数あります。以下は、pgvectorチームが提供する事前ビルドされたDockerイメージを使用したローカル設定の例です。

`docker-compose.yml`という名前のファイルを作成し、以下の内容を記述します:

```yaml
# データベースを起動するには以下のコマンドを実行:
# docker-compose up --build
version: "3"
services:
  db:
    hostname: 127.0.0.1
    image: pgvector/pgvector:pg16
    ports:
      - 5432:5432
    restart: always
    environment:
      - POSTGRES_DB=api
      - POSTGRES_USER=myuser
      - POSTGRES_PASSWORD=ChangeMe
    volumes:
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
```

`docker compose up`でPostgresコンテナを起動します。

設定したユーザーとパスワードで新しい認証情報を作成します:

<figure><img src="../../../.gitbook/assets/image (50).png" alt="" width="526"><figcaption></figcaption></figure>

`docker-compose.yml`で設定した値でノードのフィールドを入力します。例:

* ホスト: **localhost**
* データベース: **api**
* ポート: **5432**

これで完了です！Postgres Vectorの設定が完了し、使用できる状態になりました。

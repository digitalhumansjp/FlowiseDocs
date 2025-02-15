---
description: Flowise インスタンスをデータベースに接続する方法を学ぶ
---

# データベース

***

Flowise は4種類のデータベースをサポートしています:

* SQLite
* MySQL
* PostgreSQL
* MariaDB

## SQLite (デフォルト)

SQLite がデフォルトのデータベースとなります。これらのデータベースは以下の環境変数で設定できます:

```sh
DATABASE_TYPE=sqlite
DATABASE_PATH=/root/.flowise #任意の保存場所
```

`database.sqlite` ファイルが作成され、`DATABASE_PATH` で指定されたパスに保存されます。指定されていない場合、デフォルトの保存パスはホームディレクトリ -> .flowise となります。

**注意:** 環境変数が指定されていない場合、SQLite がフォールバックのデータベース選択となります。

## MySQL

```sh
DATABASE_TYPE=mysql
DATABASE_PORT=3306
DATABASE_HOST=localhost
DATABASE_NAME=flowise
DATABASE_USER=user
DATABASE_PASSWORD=123
```

## PostgreSQL

```sh
DATABASE_TYPE=postgres
DATABASE_PORT=5432
DATABASE_HOST=localhost
DATABASE_NAME=flowise
DATABASE_USER=user
DATABASE_PASSWORD=123
PGSSLMODE=require
```

## MariaDB

```bash
DATABASE_TYPE="mariadb"
DATABASE_PORT="3306"
DATABASE_HOST="localhost"
DATABASE_NAME="flowise"
DATABASE_USER="flowise"
DATABASE_PASSWORD="mypassword"
```

## Flowise データベース SQLite と MySQL/MariaDB の使用方法

{% embed url="https://youtu.be/R-6uV1Cb8I8" %}

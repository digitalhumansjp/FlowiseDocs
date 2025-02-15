---
description: Flowise インスタンスのアプリレベルのアクセス制御の設定方法を学ぶ
---

# アプリレベル

***

アプリレベルの認証は、ユーザー名とパスワードによってFlowise インスタンスを保護します。これにより、オンラインにデプロイした際に誰でもアプリにアクセスできる状態を防ぎます。

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## ユーザー名とパスワードの設定方法

### Npm

1. Flowiseをインストール

```bash
npm install -g flowise
```

2. ユーザー名とパスワードを指定してFlowiseを起動

```bash
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

3. [http://localhost:3000](http://localhost:3000) を開く

### Docker

1. `docker` フォルダに移動

```
cd docker
```

2. `.env` ファイルを作成し、`PORT`、`FLOWISE_USERNAME`、`FLOWISE_PASSWORD` を指定

```sh
PORT=3000
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```

3. `docker-compose.yml` ファイルに `FLOWISE_USERNAME` と `FLOWISE_PASSWORD` を渡す:

```
environment:
    - PORT=${PORT}
    - FLOWISE_USERNAME=${FLOWISE_USERNAME}
    - FLOWISE_PASSWORD=${FLOWISE_PASSWORD}
```

4. `docker compose up -d` を実行
5. [http://localhost:3000](http://localhost:3000) を開く
6. `docker compose stop` でコンテナを停止できます

### Gitクローン

アプリレベルの認証を有効にするには、`packages/server` の `.env` ファイルに `FLOWISE_USERNAME` と `FLOWISE_PASSWORD` を追加します:

```
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```

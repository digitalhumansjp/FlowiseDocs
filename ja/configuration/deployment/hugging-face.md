---
description: Hugging FaceへのFlowiseのデプロイ方法を学ぶ
---

# Hugging Face

***

### 新しいスペースの作成

1. [Hugging Face](https://huggingface.co/login)にサインイン
2. 任意の名前で[新しいSpace](https://huggingface.co/new-space)を作成開始
3. **Space SDK**として**Docker**を選択し、Dockerテンプレートとして**Blank**を選択
4. **Space hardware**として**CPU basic ∙ 2 vCPU ∙ 16GB ∙ FREE**を選択
5. **Create Space**をクリック

### 環境変数の設定

1. 新しいスペースの**Settings**に移動し、**Variables and Secrets**セクションを探す
2. **New variable**をクリックし、名前を`PORT`、値を`7860`として追加
3. **Save**をクリック
4. _(オプション)_ **New secret**をクリック
5. _(オプション)_ データベース認証情報やファイルパスなどの環境変数を入力。有効なフィールドは[こちら](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example)の`.env.example`で確認できます

### Dockerfileの作成

1. filesタブで、_**+ Add file**_ボタンをクリックし、**Create a new file**をクリック（またはUpload filesを選択）
2. **Dockerfile**という名前のファイルを作成し、以下の内容を貼り付け：

```Dockerfile
FROM node:18-alpine
USER root

# Arguments that can be passed at build time
ARG FLOWISE_PATH=/usr/local/lib/node_modules/flowise
ARG BASE_PATH=/root/.flowise
ARG DATABASE_PATH=$BASE_PATH
ARG APIKEY_PATH=$BASE_PATH
ARG SECRETKEY_PATH=$BASE_PATH
ARG LOG_PATH=$BASE_PATH/logs
ARG BLOB_STORAGE_PATH=$BASE_PATH/storage

# Install dependencies
RUN apk add --no-cache git python3 py3-pip make g++ build-base cairo-dev pango-dev chromium

ENV PUPPETEER_SKIP_DOWNLOAD=true
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser

# Install Flowise globally
RUN npm install -g flowise

# Configure Flowise directories using the ARG
RUN mkdir -p $LOG_PATH $FLOWISE_PATH/uploads && chmod -R 777 $LOG_PATH $FLOWISE_PATH

WORKDIR /data

CMD ["npx", "flowise", "start"]
```

3. **Commit file to `main`**をクリックすると、アプリのビルドが開始されます。

### 完了 🎉

ビルドが完了したら、**App**タブをクリックして実行中のアプリを確認できます。

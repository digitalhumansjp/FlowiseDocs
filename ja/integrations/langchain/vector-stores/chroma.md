# Chroma

## 前提条件

1. [Docker](https://www.docker.com/)と[Git](https://git-scm.com/)をダウンロード＆インストール
2. ターミナルで[Chromaのリポジトリ](https://github.com/chroma-core/chroma)をクローン

```bash
git clone https://github.com/chroma-core/chroma.git
```

3. クローンしたChromaのディレクトリに移動

```bash
cd chroma
```

4. docker composeを実行してChromaイメージとコンテナをビルド

```bash
docker compose up -d --build
```

5. 成功すると、以下のようにdockerイメージが起動しているのが確認できます:

<figure><img src="../../../.gitbook/assets/image (4) (1) (3).png" alt="" width="390"><figcaption></figcaption></figure>

## セットアップ

| 入力             | 説明                                                                                                                                | デフォルト            |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| Document         | [Document Loader](../document-loaders/)のノードと接続可能                                                                           |                       |
| エンベッディング | [Embeddings](../embeddings/)のノードと接続可能                                                                                      |                       |
| コレクション名   | Chromaコレクション名。命名規則は[こちら](https://docs.trychroma.com/usage-guide#creating-inspecting-and-deleting-collections)を参照 |                       |
| Chroma URL       | ChromaインスタンスのURLを指定                                                                                                       | http://localhost:8000 |

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (2).png" alt="" width="238"><figcaption></figcaption></figure>

### 追加設定

FlowiseとChromaの両方をDockerで実行する場合は、追加の手順が必要です。

1. まずChromaのdockerを起動

```bash
docker compose up -d --build
```

2. Flowiseの`docker-compose.yml`を開く

```bash
cd Flowise && cd docker
```

3. ファイルを以下のように修正:

```sh
version: '3.1'

services:
    flowise:
        image: flowiseai/flowise
        restart: always
        environment:
            - PORT=${PORT}
            - FLOWISE_USERNAME=${FLOWISE_USERNAME}
            - FLOWISE_PASSWORD=${FLOWISE_PASSWORD}
            - DEBUG=${DEBUG}
            - DATABASE_PATH=${DATABASE_PATH}
            - APIKEY_PATH=${APIKEY_PATH}
            - SECRETKEY_PATH=${SECRETKEY_PATH}
            - FLOWISE_SECRETKEY_OVERWRITE=${FLOWISE_SECRETKEY_OVERWRITE}
            - LOG_PATH=${LOG_PATH}
            - LOG_LEVEL=${LOG_LEVEL}
            - EXECUTION_MODE=${EXECUTION_MODE}
        ports:
            - '${PORT}:${PORT}'
        volumes:
            - ~/.flowise:/root/.flowise
        networks:
            - flowise_net
        command: /bin/sh -c "sleep 3; flowise start"
networks:
    flowise_net:
        name: chroma_net
        external: true
```

4. Flowise dockerイメージを起動

```bash
docker compose up -d
```

5. Chroma URLについて、WindowsとMacOSの場合は[http://host.docker.internal:8000](http://host.docker.internal:8000/)を指定。Linuxベースのシステムではhost.docker.internalが利用できないため、デフォルトのdockerゲートウェイ[http://172.17.0.1:8000](http://172.17.0.1:8000/)を使用します。

<figure><img src="../../../.gitbook/assets/image (5) (5).png" alt="" width="256"><figcaption></figcaption></figure>

## リソース

* [LangChain JS Chroma](https://js.langchain.com/docs/modules/indexes/vector_stores/integrations/chroma)
* [Chroma 入門](https://docs.trychroma.com/getting-started)

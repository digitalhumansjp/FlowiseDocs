---
description: Flowiseの環境変数の設定方法について学びます
---

# 環境変数

Flowiseは様々な環境変数を使用してインスタンスを設定できます。`packages/server`フォルダ内の`.env`ファイルで以下の変数を指定できます。[.env.example](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/.env.example)ファイルを参照してください。

<table><thead><tr><th width="233">変数</th><th width="219">説明</th><th width="104">型</th><th>デフォルト値</th></tr></thead><tbody><tr><td>PORT</td><td>Flowiseが実行されるHTTPポート</td><td>Number</td><td>3000</td></tr><tr><td>FLOWISE_USERNAME</td><td>ログインユーザー名</td><td>String</td><td></td></tr><tr><td>FLOWISE_PASSWORD</td><td>ログインパスワード</td><td>String</td><td></td></tr><tr><td>FLOWISE_FILE_SIZE_LIMIT</td><td>アップロード時の最大ファイルサイズ</td><td>String</td><td><code>50mb</code></td></tr><tr><td>NUMBER_OF_PROXIES</td><td>レート制限プロキシ</td><td>Number</td><td></td></tr><tr><td>CORS_ORIGINS</td><td>クロスオリジンHTTPコールで許可されるオリジン</td><td>String</td><td></td></tr><tr><td>IFRAME_ORIGINS</td><td>iframeのsrc埋め込みで許可されるオリジン</td><td>String</td><td></td></tr><tr><td>SHOW_COMMUNITY_NODES</td><td>コミュニティによって作成されたノードを表示</td><td>Boolean: <code>true</code> または <code>false</code></td><td></td></tr><tr><td>DISABLED_NODES</td><td>無効化するノード名のカンマ区切りリスト</td><td>String</td><td></td></tr></tbody></table>

## データベース用

| 変数              | 説明                                                        | 型                                         | デフォルト値             |
| ----------------- | ----------------------------------------------------------- | ------------------------------------------ | ------------------------ |
| DATABASE_TYPE     | Flowiseデータを保存するデータベースの種類                   | Enum String: `sqlite`, `mysql`, `postgres` | `sqlite`                 |
| DATABASE_PATH     | データベースが保存される場所 (DATABASE_TYPEがsqliteの場合)  | String                                     | `your-home-dir/.flowise` |
| DATABASE_HOST     | ホストURLまたはIPアドレス (DATABASE_TYPEがsqlite以外の場合) | String                                     |                          |
| DATABASE_PORT     | データベースポート (DATABASE_TYPEがsqlite以外の場合)        | String                                     |                          |
| DATABASE_USER     | データベースユーザー名 (DATABASE_TYPEがsqlite以外の場合)    | String                                     |                          |
| DATABASE_PASSWORD | データベースパスワード (DATABASE_TYPEがsqlite以外の場合)    | String                                     |                          |
| DATABASE_NAME     | データベース名 (DATABASE_TYPEがsqlite以外の場合)            | String                                     |                          |
| DATABASE_SSL      | データベースSSLが必要 (DATABASE_TYPEがsqlite以外の場合)     | Boolean: `true` または `false`             | `false`                  |

## ストレージについて

Flowiseはデフォルトでローカルパスフォルダに以下のファイルを保存します。

* [Document Loaders](../integrations/langchain/document-loaders/)/Document Storeでアップロードされたファイル
* チャットからの画像/音声アップロード
* アシスタントからの画像/ファイル
* [Vector Upsert API](../using-flowise/api.md#vector-upsert-api)からのファイル

ユーザーは`STORAGE_TYPE`を指定してAWS S3またはローカルパスを使用できます。

<table><thead><tr><th width="227">変数</th><th width="196">説明</th><th width="131">型</th><th>デフォルト値</th></tr></thead><tbody><tr><td>STORAGE_TYPE</td><td>アップロードされたファイルのストレージタイプ。デフォルトは<code>local</code></td><td>Enum String: <code>s3</code>, <code>local</code></td><td><code>local</code></td></tr><tr><td>BLOB_STORAGE_PATH</td><td><code>STORAGE_TYPE</code>が<code>local</code>の場合にアップロードされたファイルが保存されるローカルフォルダパス</td><td>String</td><td><code>your-home-dir/.flowise/storage</code></td></tr><tr><td>S3_STORAGE_BUCKET_NAME</td><td><code>STORAGE_TYPE</code>が<code>s3</code>の場合にアップロードされたファイルを保持するバケット名</td><td>String</td><td></td></tr><tr><td>S3_STORAGE_ACCESS_KEY_ID</td><td>AWSアクセスキー</td><td>String</td><td></td></tr><tr><td>S3_STORAGE_SECRET_ACCESS_KEY</td><td>AWSシークレットキー</td><td>String</td><td></td></tr><tr><td>S3_STORAGE_REGION</td><td>S3バケットのリージョン</td><td>String</td><td></td></tr><tr><td>S3_ENDPOINT_URL</td><td>カスタムS3エンドポイント(オプション)</td><td>String</td><td></td></tr><tr><td>S3_FORCE_PATH_STYLE</td><td>S3パススタイルを強制(オプション)</td><td>Boolean</td><td>false</td></tr></tbody></table>

## デバッグとログについて

| 変数      | 説明                         | 型                                               | デフォルト値                   |
| --------- | ---------------------------- | ------------------------------------------------ | ------------------------------ |
| DEBUG     | コンポーネントからログを出力 | Boolean                                          |                                |
| LOG_PATH  | ログファイルが保存される場所 | String                                           | `Flowise/packages/server/logs` |
| LOG_LEVEL | 異なるレベルのログ           | Enum String: `error`, `info`, `verbose`, `debug` | `info`                         |

`DEBUG`: trueに設定すると、ターミナル/コンソールにログを出力します:

<figure><img src="../.gitbook/assets/image (3) (3).png" alt=""><figcaption></figcaption></figure>

`LOG_LEVEL`: ロガーに保存される異なるログレベル。`error`、`info`、`verbose`、または`debug`を指定できます。デフォルトでは`info`に設定されており、`logger.info`のみがログファイルに保存されます。完全な詳細が必要な場合は、`debug`に設定してください。

<figure><img src="../.gitbook/assets/image (2) (4).png" alt=""><figcaption><p><strong>server-requests.log.jsonl - Flowiseに送信されたすべてのリクエストをログに記録</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><strong>server.log - Flowiseの一般的なアクションをログに記録</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (4).png" alt=""><figcaption><p><strong>server-error.log - スタックトレース付きのエラーをログに記録</strong></p></figcaption></figure>

### ログのS3ストリーミング

`STORAGE_TYPE`環境変数が`s3`に設定されている場合、ログは自動的にS3にストリーミングされて保存されます。新しいログファイルは1時間ごとに作成され、デバッグが容易になります。

## 認証情報について

Flowiseは、暗号化キーを使用して第三者のAPIキーを暗号化された認証情報として保存します。

デフォルトでは、アプリケーション起動時にランダムな暗号化キーが生成され、ファイルパスに保存されます。この暗号化キーは、チャットフロー内で使用される認証情報（OpenAI APIキー、Pinecone APIキーなど）を復号化するために毎回取得されます。

AWS Secret Managerを使用して暗号化キーを保存するように設定することもできます。

| 変数                        | 説明                                       | 型                          | デフォルト値              |
| --------------------------- | ------------------------------------------ | --------------------------- | ------------------------- |
| SECRETKEY_STORAGE_TYPE      | 暗号化キーの保存方法                       | Enum String: `local`, `aws` | `local`                   |
| SECRETKEY_PATH              | 暗号化キーが保存されるローカルファイルパス | String                      | `Flowise/packages/server` |
| FLOWISE_SECRETKEY_OVERWRITE | 既存のキーの代わりに使用する暗号化キー     | String                      |                           |
| SECRETKEY_AWS_ACCESS_KEY    |                                            | String                      |                           |
| SECRETKEY_AWS_SECRET_KEY    |                                            | String                      |                           |
| SECRETKEY_AWS_REGION        |                                            | String                      |                           |

何らかの理由で暗号化キーが再生成されたり、保存パスが変更されたりすると、<mark style="color:red;">認証情報を復号化できません</mark>というようなエラーが発生することがあります。

これを避けるために、`FLOWISE_SECRETKEY_OVERWRITE`として独自の暗号化キーを設定できます。これにより、毎回同じ暗号化キーが使用されます。形式に制限はなく、任意のテキストや`FLOWISE_PASSWORD`と同じものを設定できます。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
UIから返される認証情報APIキーは、設定した元のAPIキーと同じ長さではありません。これはネットワークスプーフィングを防ぐための偽のプレフィックス文字列であり、そのためUIにAPIキーを返していません。ただし、チャットフローとのやり取り中は正しいAPIキーが取得され使用されます。
{% endhint %}

## モデルについて

場合によっては、既存のチャットモデルやLLMノードでカスタムモデルを使用したり、特定のモデルへのアクセスを制限したりすることがあります。

デフォルトでは、Flowise は[ここ](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/models.json)からモデルリストを取得します。ただし、ユーザーは独自の`models.json`ファイルを作成してファイルパスを指定できます：

<table><thead><tr><th width="164">変数</th><th width="196">説明</th><th width="78">型</th><th>デフォルト値</th></tr></thead><tbody><tr><td>MODEL_LIST_CONFIG_JSON</td><td>独自の<code>models.json</code>設定ファイルからモデルリストを読み込むためのリンク</td><td>String</td><td><a href="https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json">https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json</a></td></tr></tbody></table>

## APIキーについて

ユーザーは[API](../using-flowise/api.md)認証のためにFlowise内で複数のAPIキーを作成できます。デフォルトでは、キーはJSONファイルとしてローカルファイルパスに保存されます。以下の環境変数を使用して動作を変更できます。

| 変数                | 説明                                                                     | 型                        | デフォルト値              |
| ------------------- | ------------------------------------------------------------------------ | ------------------------- | ------------------------- |
| APIKEY_STORAGE_TYPE | APIキーの保存方法                                                        | Enum string: `json`, `db` | `json`                    |
| APIKEY_PATH         | `APIKEY_STORAGE_TYPE`が未指定または`json`の場合にAPIキーが保存される場所 | String                    | `Flowise/packages/server` |

ストレージタイプとして`db`を使用すると、APIキーはローカルJSONファイルではなくデータベースに保存されます。

<figure><img src="../.gitbook/assets/image (254).png" alt=""><figcaption><p>Flowise APIキー</p></figcaption></figure>

## ビルトインおよび外部依存関係について

Flowise内には、JavaScriptコードを実行できる特定のノード/機能があります。セキュリティ上の理由から、デフォルトでは特定の依存関係のみが許可されています。以下の環境変数を設定することで、ビルトインモジュールと外部モジュールの制限を解除することができます：

| 変数                       | 説明                                           |        |
| -------------------------- | ---------------------------------------------- | ------ |
| TOOL_FUNCTION_BUILTIN_DEP  | ツール機能で使用するNodeJSビルトインモジュール | String |
| TOOL_FUNCTION_EXTERNAL_DEP | ツール機能で使用する外部モジュール             | String |

{% code title=".env" %}
```bash
# すべてのビルトインモジュールの使用を許可
TOOL_FUNCTION_BUILTIN_DEP=*

# fsのみ使用を許可
TOOL_FUNCTION_BUILTIN_DEP=fs

# cryptoとfsのみ使用を許可
TOOL_FUNCTION_BUILTIN_DEP=crypto,fs

# 外部npmモジュールの使用を許可
TOOL_FUNCTION_EXTERNAL_DEP=axios,moment
```
{% endcode %}

## 環境変数の設定例：

### NPM

npxを使用してFlowiseを実行する際に、これらの変数をすべて設定できます。例：

```
npx flowise start --PORT=3000 --DEBUG=true
```

### Docker

```
docker run -d -p 5678:5678 flowise \
 -e DATABASE_TYPE=postgresdb \
 -e DATABASE_PORT=<POSTGRES_PORT> \
 -e DATABASE_HOST=<POSTGRES_HOST> \
 -e DATABASE_NAME=<POSTGRES_DATABASE_NAME> \
 -e DATABASE_USER=<POSTGRES_USER> \
 -e DATABASE_PASSWORD=<POSTGRES_PASSWORD> \
```

### Docker Compose

`docker`フォルダ内の`.env`ファイルでこれらの変数をすべて設定できます。[.env.example](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example)ファイルを参照してください。

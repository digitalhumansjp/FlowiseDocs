# 本番環境での実行

## モード

本番環境で実行する場合、以下の設定で[キュー](running-flowise-using-queue.md)モードを使用することを強く推奨します：

* ロードバランシングを行う2台以上のメインサーバー、各サーバーは最低2 CPU 4GB RAM
* 2台以上のワーカー、各ワーカーは最低1 CPU 2GB RAM

トラフィックと処理量に応じて自動スケーリングを設定できます。

## データベース

デフォルトでは、FlowiseはSQLiteをデータベースとして使用します。ただし、スケールする場合はPostgreSQLの使用を推奨します。

## ストレージ

現在、Flowiseは[AWS S3](https://aws.amazon.com/s3/)のみをサポートしており、より多くのBlobストレージプロバイダーのサポートを計画しています。これにより、ファイルとログをローカルファイルパスではなくS3に保存できます。[#for-storage](environment-variables.md#for-storage "mention")を参照してください。

## 暗号化

FlowiseはOpenAI APIキーなどの認証情報の暗号化/復号化に暗号化キーを使用します。本番環境では、セキュリティ制御とキーローテーションを向上させるために[AWS Secret Manager](https://aws.amazon.com/secrets-manager/)の使用を推奨します。[#for-credentials](environment-variables.md#for-credentials "mention")を参照してください。

## APIキーストレージ

ユーザーは[API](../using-flowise/api.md)認証のために、Flowise内で複数のAPIキーを作成できます。デフォルトでは、キーはJSONファイルとしてローカルファイルパスに保存されます。ただし、複数のインスタンスがある場合、各インスタンスが新しいJSONファイルを作成し、混乱を招く可能性があります。代わりにデータベースに保存する動作に変更できます。[#for-flowise-api-keys](environment-variables.md#for-flowise-api-keys "mention")を参照してください。

## レート制限

クラウド/オンプレミスにデプロイする場合、ほとんどのインスタンスはプロキシ/ロードバランサーの背後にあります。リクエストのIPアドレスがロードバランサー/リバースプロキシのIPになる可能性があり、レート制限が事実上グローバルなものとなり、制限に達するか`undefined`になるとすべてのリクエストがブロックされます。正しい`NUMBER_OF_PROXIES`を設定することでこの問題を解決できます。[#rate-limit-setup](rate-limit.md#rate-limit-setup "mention")を参照してください。

## 負荷テスト

Artilleryを使用してデプロイされたFlowiseアプリケーションの負荷テストを行うことができます。サンプルスクリプトは[こちら](https://github.com/FlowiseAI/Flowise/blob/main/artillery-load-test.yml)で確認できます。

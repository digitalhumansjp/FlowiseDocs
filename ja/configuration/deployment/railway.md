---
description: RailwayへのFlowiseのデプロイ方法を学ぶ
---

# Railway

***

1. 以下の事前ビルドされた[テンプレート](https://railway.app/template/pn4G8S?referralCode=WVNPD9)をクリック
2. Deploy Nowをクリック

<figure><img src="../../.gitbook/assets/image (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

3. 任意のリポジトリ名に変更し、Deployをクリック

<figure><img src="../../.gitbook/assets/image (2) (1) (2) (1).png" alt="" width="375"><figcaption></figcaption></figure>

4. 成功すると、デプロイされたURLが表示されます

<figure><img src="../../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

5. 認証を追加するには、Variablesタブに移動して以下を追加：

* FLOWISE_USERNAME
* FLOWISE_PASSWORD

<figure><img src="../../.gitbook/assets/image (15) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

6. 設定可能な環境変数の一覧は[environment-variables.md](../environment-variables.md "mention")を参照してください

これで完了です！RailwayにFlowiseがデプロイされました[🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)

## 永続ボリューム

Railwayで実行されるサービスのデフォルトのファイルシステムは一時的なものです。Flowiseのデータはデプロイや再起動時に保持されません。この問題を解決するために、[Railway Volume](https://docs.railway.app/reference/volumes)を使用できます。

手順を簡単にするために、ボリュームがマウントされたRailwayテンプレートを用意しています：[https://railway.app/template/nEGbjR](https://railway.app/template/nEGbjR)

Deployをクリックし、以下のように環境変数を入力するだけです：

* DATABASE_PATH - `/opt/railway/.flowise`
* APIKEY_PATH - `/opt/railway/.flowise`
* LOG_PATH - `/opt/railway/.flowise/logs`
* SECRETKEY_PATH - `/opt/railway/.flowise`
* BLOB_STORAGE_PATH - `/opt/railway/.flowise/storage`

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="420"><figcaption></figcaption></figure>

Flowiseでフローを作成して保存してみてください。その後、サービスを再起動するか再デプロイしても、以前保存したフローを確認できるはずです。

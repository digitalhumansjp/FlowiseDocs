---
description: ZeaburへのFlowiseのデプロイ方法を学ぶ
---

# Zeabur

***

{% hint style="warning" %}
Zeaburによって作成された以下のテンプレートは古いバージョン（2024-01-24時点）であることにご注意ください。
{% endhint %}

1. 以下の事前ビルドされた[テンプレート](https://zeabur.com/templates/2JYZTR)または下のボタンをクリックします。

[![Deploy on Zeabur](https://zeabur.com/button.svg)](https://zeabur.com/templates/2JYZTR)

2. Deployをクリック

<figure><img src="../../.gitbook/assets/zeabur/1.png" alt="zeabur template"><figcaption></figcaption></figure>

3. 任意のリージョンを選択して続行

<figure><img src="../../.gitbook/assets/zeabur/2.png" alt="select region"><figcaption></figcaption></figure>

4. Zeaburのダッシュボードにリダイレクトされ、デプロイプロセスが表示されます

<figure><img src="../../.gitbook/assets/zeabur/3.png" alt="deployment process"><figcaption></figcaption></figure>

5. 認証を追加するには、Variablesタブに移動して以下を追加：

* FLOWISE_USERNAME
* FLOWISE_PASSWORD

<figure><img src="../../.gitbook/assets/zeabur/4.png" alt="authorization"><figcaption></figcaption></figure>

6. 設定可能な環境変数の一覧は[environment-variables.md](../environment-variables.md "mention")を参照してください

これで完了です！ZeaburにFlowiseがデプロイされました[🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)

## 永続ボリューム

Zeaburは自動的に永続ボリュームを作成するため、この点について心配する必要はありません。

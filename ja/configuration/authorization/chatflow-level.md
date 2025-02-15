---
description: Flowise インスタンスのチャットフローレベルのアクセス制御の設定方法を学ぶ
---

# チャットフローレベル

***

チャットフロー/エージェントフローを構築した後、デフォルトではフローは公開されています。チャットフロー ID にアクセスできる人は誰でも、埋め込みや API を通じて予測を実行できます。

特定の人々だけがアクセスして操作できるようにしたい場合は、そのチャットフロー専用の API キーを割り当てることができます。

## API キー

ダッシュボードで API キーセクションに移動すると、DefaultKey が作成されているのが確認できます。キーの追加や削除も可能です。

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## チャットフロー

チャットフローに移動し、チャットフローを保護するために使用したい API キーを選択できます。

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

API キーを割り当てた後は、HTTP 呼び出し時に正しい API キーを含む Authorization ヘッダーを提供した場合のみ、チャットフロー API にアクセスできます。

```json
"Authorization": "Bearer <your-api-key>"
```

POSTMAN を使用した API 呼び出しの例

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

`APIKEY_PATH` 環境変数を指定することで、API キーを保存する場所を指定できます。詳細は [environment-variables.md](../environment-variables.md "mention") を参照してください。

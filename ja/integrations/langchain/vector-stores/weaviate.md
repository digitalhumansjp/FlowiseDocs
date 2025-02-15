---
description: >-
  スケーラブルなオープンソースベクトルデータベースであるWeaviateを使用して、エンベッドされたデータをアップサートし、類似性検索またはMMR検索を実行します。
---

# Weaviate

<figure><img src="../../../.gitbook/assets/image (165).png" alt="" width="295"><figcaption><p>Weaviateノード</p></figcaption></figure>

## フィルタリング

Weaviateはフィルタリングに関して以下の[構文](https://weaviate.io/developers/weaviate/search/filters)をサポートしています:

**UI**

<figure><img src="../../../.gitbook/assets/image (5) (1) (1).png" alt="" width="227"><figcaption></figcaption></figure>

**API**

```json
"overrideConfig": {
    "weaviateFilter": {
        "where": {
            "operator": "Equal",
            "path": [
                "test"
            ],
            "valueText": "key"
        }
    }
}
```

## リソース

* [LangchainJS Weaviate](https://js.langchain.com/v0.1/docs/integrations/vectorstores/weaviate/#usage-query-documents)
* [Weaviateフィルタリング](https://weaviate.io/developers/weaviate/search/filters)

{% hint style="info" %}
このセクションは作業中です。このセクションの完成にご協力いただける方を募集しています。[コントリビューションガイド](../../../contributing/)をご確認の上、ご協力をお願いいたします。
{% endhint %}

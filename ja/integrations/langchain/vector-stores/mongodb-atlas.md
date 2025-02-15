---
description: >-
  マネージドクラウドMongoDBデータベースであるMongoDB Atlasを使用して、エンベッドされたデータをアップサートし、クエリに対して類似性検索またはMMR検索を実行します。
---

# MongoDB Atlas

<figure><img src="../../../.gitbook/assets/image (161).png" alt="" width="308"><figcaption><p>MongoDB Atlasノード</p></figcaption></figure>

### クラスター設定[​](https://js.langchain.com/docs/integrations/vectorstores/mongodb_atlas/#initial-cluster-configuration) <a href="#initial-cluster-configuration" id="initial-cluster-configuration"></a>

MongoDB Atlasクラスターを設定するには、[MongoDB Atlas](https://www.mongodb.com/)のウェブサイトにアクセスし、アカウントをお持ちでない場合は登録してください。プロンプトが表示されたら、データベースセクションに表示されるクラスターを作成し名前を付けます。次に、「**Browse Collections**」を選択して、新しいコレクションを作成するか、提供されているサンプルデータからコレクションを使用します。

{% hint style="warning" %}
作成するクラスターがバージョン7.0以上であることを確認してください。
{% endhint %}

### インデックスの作成

クラスターを設定した後、次のステップは検索対象のコレクションフィールドのインデックスを作成することです。

1. **Atlas Search**タブに移動し、**Create Search Index**をクリックします。
2. **Atlas Vector Search - JSON Editor**を選択し、適切なデータベースとコレクションを選択して、以下をテキストボックスに貼り付けます:

```json
{
  "fields": [
    {
      "numDimensions": 1536,
      "path": "embedding",
      "similarity": "euclidean",
      "type": "vector"
    }
  ]
}
```

`numDimensions`プロパティが使用するエンベッディングの次元数と一致していることを確認してください。例えば、Cohereエンベッディングは通常1024次元、OpenAIエンベッディングはデフォルトで1536次元です。

**注意:** ベクトルストアは以下のようなデフォルト値を想定しています:

* インデックス名は`default`
* コレクションフィールド名は`embedding`
* 生テキストフィールド名は`text`

上記の例のように、インデックスとコレクションスキーマに一致するフィールド名でベクトルストアを初期化してください。

これが完了したら、インデックスのビルドに進みます。

{% hint style="info" %}
このセクションは作業中です。このセクションの完成にご協力いただける方を募集しています。[コントリビューションガイド](../../../contributing/)をご確認の上、ご協力をお願いいたします。
{% endhint %}

### Flowise設定

MongoDB Atlas Vector Storeをドラッグ＆ドロップし、新しい認証情報を追加します。MongoDB Atlasダッシュボードから提供される接続文字列を使用します:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

残りのフィールドを入力します:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="252"><figcaption></figcaption></figure>

追加パラメータからより詳細な設定も可能です:

<figure><img src="../../../.gitbook/assets/image (164).png" alt="" width="518"><figcaption></figcaption></figure>

---
description: LangChain レコードマネージャーノード
---

# レコードマネージャー

***

レコードマネージャーは、インデックス付けされたドキュメントを追跡し、[ベクトルストア](vector-stores/)内のベクトル埋め込みの重複を防ぎます。

ドキュメントチャンクがアップサートされる際、各チャンクは[SHA-1](https://github.com/emn178/js-sha1)アルゴリズムを使用してハッシュ化されます。これらのハッシュはレコードマネージャーに保存されます。既存のハッシュが存在する場合、埋め込みとアップサートのプロセスはスキップされます。

場合によっては、インデックス付けされる新しいドキュメントと同じソースから派生した既存のドキュメントを削除したい場合があります。そのため、レコードマネージャーには3つのクリーンアップモードがあります：

{% tabs %}
{% tab title="Incremental" %}
複数のドキュメントをアップサートする際に、現在のアップサートプロセスの一部ではない既存のドキュメントの削除を防ぎたい場合は、**Incremental**クリーンアップモードを使用します。

1. `Incremental`クリーンアップと`source`をSourceId Keyとするレコードマネージャーを作成します

<div align="left"><figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="264"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="410"><figcaption></figcaption></figure></div>

2. 以下の2つのドキュメントを用意します：

| テキスト | メタデータ       |
| -------- | ---------------- |
| Cat      | `{source:"cat"}` |
| Dog      | `{source:"dog"}` |

<div align="left"><figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1).png" alt="" width="202"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (2).png" alt="" width="231"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. アップサート後、2つのドキュメントがアップサートされたことが確認できます：

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2).png" alt="" width="433"><figcaption></figcaption></figure>

4. ここで、**Dog**ドキュメントを削除し、**Cat**を**Cats**に更新すると、以下のようになります：

<figure><img src="../../.gitbook/assets/image (13) (2).png" alt="" width="425"><figcaption></figcaption></figure>

* 元の**Cat**ドキュメントは削除されます
* **Cats**という新しいドキュメントが追加されます
* **Dog**ドキュメントは変更されません
* ベクトルストアに残っているベクトル埋め込みは**Cats**と**Dog**です

<figure><img src="../../.gitbook/assets/image (15) (1) (1).png" alt="" width="448"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Full" %}
複数のドキュメントをアップサートする際、**Full**クリーンアップモードは現在のアップサートプロセスの一部ではないベクトル埋め込みを自動的に削除します。

1. `Full`クリーンアップを持つレコードマネージャーを作成します。FullクリーンアップモードではSourceId Keyは必要ありません。

<div align="left"><figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="264"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (17) (1) (1).png" alt="" width="407"><figcaption></figcaption></figure></div>

2. 以下の2つのドキュメントを用意します：

| テキスト | メタデータ       |
| -------- | ---------------- |
| Cat      | `{source:"cat"}` |
| Dog      | `{source:"dog"}` |

<div align="left"><figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1).png" alt="" width="202"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (2).png" alt="" width="231"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. アップサート後、2つのドキュメントがアップサートされたことが確認できます：

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2).png" alt="" width="433"><figcaption></figcaption></figure>

4. ここで、**Dog**ドキュメントを削除し、**Cat**を**Cats**に更新すると、以下のようになります：

<figure><img src="../../.gitbook/assets/image (18) (1) (1).png" alt="" width="430"><figcaption></figcaption></figure>

* 元の**Cat**ドキュメントは削除されます
* **Cats**という新しいドキュメントが追加されます
* **Dog**ドキュメントは削除されます
* ベクトルストアに残っているベクトル埋め込みは**Cats**のみです

<figure><img src="../../.gitbook/assets/image (19) (1) (1).png" alt="" width="527"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="None" %}
クリーンアップは実行されません
{% endtab %}
{% endtabs %}

現在利用可能なレコードマネージャーノードは以下の通りです：

* SQLite
* MySQL
* PostgresQL

## リソース

* [LangChain Indexing - 仕組みについて](https://js.langchain.com/docs/how_to/indexing/#how-it-works)

---
description: Spider - 最速のオープンソースウェブスクレイパー＆クローラーでウェブをスクレイプ＆クロールする
---

# Spider ウェブスクレイパー/クローラー

<figure><img src="../../../.gitbook/assets/spider.png" alt="Spiderノード" width="365"><figcaption><p>Spider ウェブスクレイパー/クローラーノード</p></figcaption></figure>

[Spider](https://spider.cloud/?ref=flowise)は、LLM対応のデータを返す最速のオープンソースウェブスクレイパー＆クローラーです。このノードを使用するには、[Spider.cloud](https://spider.cloud/?ref=flowise)からAPIキーを取得する必要があります。

## はじめに

1. [Spider.cloud](https://spider.cloud/?ref=flowise)のウェブサイトにアクセスし、無料アカウントにサインアップします。
2. [APIキー](https://spider.cloud/api-keys)に移動して新しいAPIキーを作成します。
3. APIキーをコピーし、SpiderノードのCredentialフィールドに貼り付けます。

## スクレイプ＆クロール

1. モードドロップダウンで「Scrape」または「Crawl」を選択します。
2. 「Web Page URL」フィールドにスクレイプまたはクロールしたいURLを入力します。
3. 「Crawl」を選択した場合、「Limit」フィールドにクロールしたい最大ページ数を入力します。値が入力されていないか0の場合、クローラーはすべてのページをクロールします。

## 例

<figure><img src="../../../.gitbook/assets/spider_example_usage.png" alt="Spiderノードの使用例" width="365"><figcaption><p>Spiderノードの使用例</p></figcaption></figure>

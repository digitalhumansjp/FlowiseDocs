---
description: ウェブサイトのスクレイピング、アップサート、クエリの方法を学ぶ
---

# Webスクレイプ Q&A

***

ウェブサイト（ストア、Eコマースサイト、ブログなど）があり、そのウェブサイトの関連リンクをすべてスクレイプして、LLMにウェブサイトに関する質問に回答させたい場合があります。このチュートリアルでは、その方法について説明します。

マーケットプレイステンプレートから**WebPage QnA**という例のフローを見つけることができます。

## セットアップ

**Cheerio Webスクレイパー**ノードを使用して指定されたURLからリンクをスクレイプし、**HtmlToMarkdownテキストスプリッター**を使用してスクレイプしたコンテンツを小さな部分に分割します。

<figure><img src="../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

何も指定しない場合、デフォルトでは指定されたURLのページのみがスクレイプされます。残りの関連リンクをクロールしたい場合は、Cheerio Webスクレイパーの**追加パラメータ**をクリックします。

## 1. 複数ページのクロール

1. **Get Relative Links Method**で`Web Crawl`または`Scrape XML Sitemap`を選択します。
2. **Get Relative Links Limit**に`0`を入力して、提供されたURLから利用可能なすべてのリンクを取得します。

<figure><img src="../.gitbook/assets/image (87).png" alt="" width="563"><figcaption></figcaption></figure>

### リンクの管理（オプション）

1. クロールしたいURLを入力します。
2. **Fetch Links**をクリックして、**追加パラメータ**の**Get Relative Links Method**と**Get Relative Links Limit**の入力に基づいてリンクを取得します。
3. **Crawled Links**セクションで、**赤いゴミ箱アイコン**をクリックして不要なリンクを削除します。
4. 最後に、**Save**をクリックします。

<figure><img src="../.gitbook/assets/image (88).png" alt="" width="563"><figcaption></figcaption></figure>

## 2. アップサート

1. 右上隅に緑色のボタンが表示されます：

<figure><img src="../.gitbook/assets/Untitled (2).png" alt=""><figcaption></figcaption></figure>

2. Pineconeにデータをアップサートできるダイアログが表示されます：

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

**注意：**バックグラウンドでは、以下のアクションが実行されます：

* Cheerio Webスクレイパーを使用してすべてのHTMLデータをスクレイプ
* スクレイプしたすべてのデータをHTMLからMarkdownに変換し、分割
* 分割されたデータをループ処理し、OpenAI Embeddingsを使用してベクトルエンベッディングに変換
* ベクトルエンベッディングをPineconeにアップサート

3. [Pineconeコンソール](https://app.pinecone.io)で、追加された新しいベクトルを確認できます。

<figure><img src="../.gitbook/assets/web-scrape-pinecone.png" alt=""><figcaption></figcaption></figure>

## 3. クエリ

クエリは比較的単純です。データがベクトルデータベースにアップサートされたことを確認したら、チャットで質問を開始できます：

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

Conversational Retrieval QA Chainの追加パラメータで、2つのプロンプトを指定できます：

* **Rephrase Prompt：**過去の会話履歴を考慮して質問を言い換えるために使用
* **Response Prompt：**言い換えられた質問を使用して、ベクトルデータベースからコンテキストを取得し、最終的な応答を返す

<figure><img src="../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
詳細なレスポンスプロンプトメッセージを指定することをお勧めします。例えば、AIの名前、回答する言語、回答が見つからない場合の応答（幻覚を防ぐため）を指定できます。
{% endhint %}

Return Source Documentsオプションをオンにして、AIの応答の元となったドキュメントチャンクのリストを返すこともできます。

<figure><img src="../.gitbook/assets/Untitled (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

## 追加のWebスクレイピング

Cheerio Webスクレイパー以外にも、Webスクレイピングを実行できる他のノードがあります：

* **Puppeteer：**PuppeteerはヘッドレスChromeまたはChromiumを制御するための高レベルAPIを提供するNode.jsライブラリです。JavaScriptでのレンダリングが必要な動的Webページからのデータ抽出を含む、Webページの操作を自動化するためにPuppeteerを使用できます。
* **Playwright：**PlaywrightはChromium、Firefox、WebKitを含む複数のブラウザエンジンを制御するための高レベルAPIを提供するNode.jsライブラリです。JavaScriptでのレンダリングが必要な動的Webページからのデータ抽出を含む、Webページの操作を自動化するためにPlaywrightを使用できます。
* **Apify：**[Apify](https://apify.com/)は、様々なWebスクレイピング、クローリング、データ抽出のユースケース向けに1000以上の既製アプリケーション（_Actors_と呼ばれる）の[エコシステム](https://apify.com/store)を提供するWebスクレイピングとデータ抽出のためのクラウドプラットフォームです。

<figure><img src="../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
同じロジックはWebスクレイピングに限らず、あらゆるドキュメントのユースケースに適用できます！
{% endhint %}

パフォーマンスを改善する方法についての提案がありましたら、[貢献](../contributing/)をお待ちしています！

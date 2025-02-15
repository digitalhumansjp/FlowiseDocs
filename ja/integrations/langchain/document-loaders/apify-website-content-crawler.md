---
description: Apify Website Content Crawlerからデータを読み込む
---

# Apify Website Content Crawler

[Apify](https://apify.com/)は、Actorsと呼ばれる1000以上のクラウドツールを提供するウェブスクレイピングとデータ抽出プラットフォームです。

[Website Content Crawler](https://apify.com/apify/website-content-crawler) Actorは、ウェブサイトを深くクロールし、クッキーモーダル、フッター、ナビゲーションなどを削除してHTMLをクリーンアップし、HTMLをMarkdownに変換できます。このMarkdownは、セマンティック検索や検索拡張生成（RAG）のためにベクトルデータベースに保存できます。

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="266"><figcaption><p>Apify Website Content Crawlerノード</p></figcaption></figure>

## ウェブサイト全体のクロール

1. _(オプション)_ [**テキストスプリッター**](../text-splitters/)を接続します。
2. Apify APIを接続します（[Apify APIトークン](https://my.apify.com/account#/integrations)で新しい認証情報を作成）。
3. クローラーが開始する1つまたは複数のURL（カンマ区切り）を入力します（例：`https://docs.flowiseai.com/`）。
4. クローラータイプを選択します。詳細は[Website Content Crawlerのドキュメント](https://apify.com/apify/website-content-crawler/input-schema#crawlerType)を参照してください。
5. _(オプション)_ 最大クロール深度や最大クロールページ数などの追加パラメータを指定します。

## 出力

ウェブサイトのコンテンツをドキュメントとして読み込みます。

## リソース

* [Apify-Flowise統合](https://docs.apify.com/platform/integrations/flowise)
* [Website Content Crawler](https://apify.com/apify/website-content-crawler)

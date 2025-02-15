---
description: >-
  主要なフルマネージドホステッドベクトルデータベースであるPineconeを使用して、エンベッドされたデータをアップサートし、クエリに対して類似性検索を実行します。
---

# Pinecone

## 前提条件

1. [Pinecone](https://app.pinecone.io/)のアカウントを登録
2. **Create index**をクリック

<figure><img src="../../../.gitbook/assets/pinecone_1.png" alt=""><figcaption></figcaption></figure>

3. 必要なフィールドを入力:
   - **Index Name**、作成するインデックスの名前（例: "flowise-test"）
   - **Dimensions**、インデックスに挿入するベクトルのサイズ（例: 1536）

<figure><img src="../../../.gitbook/assets/pinecone_2.png" alt="" width="527"><figcaption></figcaption></figure>

4. **Create Index**をクリック

## セットアップ

1. **API Key**を取得/作成

<figure><img src="../../../.gitbook/assets/pinecone_3.png" alt=""><figcaption></figcaption></figure>

2. キャンバスに新しい**Pinecone**ノードを追加し、パラメータを入力:
    - Pineconeインデックス
    - Pineconeネームスペース（オプション）

<figure><img src="../../../.gitbook/assets/pinecone_4.png" alt="" width="279"><figcaption></figcaption></figure>

3. 新しいPinecone認証情報を作成 -> **API Key**を入力

<figure><img src="../../../.gitbook/assets/pinecone_5.png" alt="" width="563"><figcaption></figcaption></figure>

4. キャンバスに追加のノードを追加してアップサート処理を開始
   - **Document**は[**Document Loader**](../document-loaders/)カテゴリの任意のノードと接続可能
   - **エンベッディング**は[**Embeddings**](../embeddings/)カテゴリの任意のノードと接続可能

<figure><img src="../../../.gitbook/assets/pinecone_6.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/pinecone_7.png" alt=""><figcaption></figcaption></figure>

5. [Pineconeダッシュボード](https://app.pinecone.io)でデータが正常にアップサートされたことを確認:

<figure><img src="../../../.gitbook/assets/pinecone_8.png" alt=""><figcaption></figcaption></figure>

6.

## リソース

- LangChain Pineconeベクトルストア統合
  - [Python](https://python.langchain.com/v0.2/docs/integrations/providers/pinecone/)
  - [NodeJS](https://js.langchain.com/v0.2/docs/integrations/vectorstores/pinecone)
- [Pinecone LangChain統合](https://docs.pinecone.io/integrations/langchain)
- [Pinecone Flowise統合](https://docs.pinecone.io/integrations/flowise)
- [Pinecone公式クライアント](https://docs.pinecone.io/reference/pinecone-clients)

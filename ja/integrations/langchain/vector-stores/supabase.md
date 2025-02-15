# Supabase

## 前提条件

1. [Supabase](https://supabase.com/)のアカウントを登録
2. **New project**をクリック

<figure><img src="../../../.gitbook/assets/image (8) (2) (1).png" alt=""><figcaption></figcaption></figure>

3. 必要なフィールドを入力

| フィールド名          | 説明                                      |
| --------------------- | ----------------------------------------- |
| **Name**              | 作成するプロジェクトの名前（例: Flowise） |
| **Database Password** | postgresデータベースのパスワード          |

<figure><img src="../../../.gitbook/assets/image (25) (1) (1).png" alt=""><figcaption></figcaption></figure>

4. **Create new project**をクリックしてプロジェクトのセットアップが完了するまで待機
5. **SQL Editor**をクリック

<figure><img src="../../../.gitbook/assets/image (7) (2).png" alt=""><figcaption></figcaption></figure>

6. **New query**をクリック

<figure><img src="../../../.gitbook/assets/image (36) (1).png" alt=""><figcaption></figcaption></figure>

7. 以下のSQLクエリをコピー＆ペーストし、`Ctrl + Enter`または**RUN**をクリックして実行。テーブル名と関数名をメモしておきます。

* **テーブル名**: `documents`
* **クエリ名**: `match_documents`

```plsql
-- エンベッディングベクトルを扱うためのpgvector拡張を有効化
create extension vector;

-- ドキュメントを保存するテーブルを作成
create table documents (
  id bigserial primary key,
  content text, -- Document.pageContentに対応
  metadata jsonb, -- Document.metadataに対応
  embedding vector(1536) -- OpenAIエンベッディングでは1536、必要に応じて変更
);

-- ドキュメントを検索する関数を作成
create function match_documents (
  query_embedding vector(1536),
  match_count int DEFAULT null,
  filter jsonb DEFAULT '{}'
) returns table (
  id bigint,
  content text,
  metadata jsonb,
  similarity float
)
language plpgsql
as $$
#variable_conflict use_column
begin
  return query
  select
    id,
    content,
    metadata,
    1 - (documents.embedding <=> query_embedding) as similarity
  from documents
  where metadata @> filter
  order by documents.embedding <=> query_embedding
  limit match_count;
end;
$$;
```

[Record Manager](../record-managers.md)を使用してアップサートを追跡し重複を防ぐ場合、Record Managerは各エンベッディングにランダムなUUIDを生成するため、idカラムのエンティティをtextに変更する必要があります:

```sql
-- エンベッディングベクトルを扱うためのpgvector拡張を有効化
create extension vector;

-- ドキュメントを保存するテーブルを作成
create table documents (
  id text primary key, -- TEXTに変更
  content text,
  metadata jsonb,
  embedding vector(1536)
);

-- ドキュメントを検索する関数を作成
create function match_documents (
  query_embedding vector(1536),
  match_count int DEFAULT null,
  filter jsonb DEFAULT '{}'
) returns table (
  id text, -- TEXTに変更
  content text,
  metadata jsonb,
  similarity float
)
language plpgsql
as $$
#variable_conflict use_column
begin
  return query
  select
    id,
    content,
    metadata,
    1 - (documents.embedding <=> query_embedding) as similarity
  from documents
  where metadata @> filter
  order by documents.embedding <=> query_embedding
  limit match_count;
end;
$$;
```

<figure><img src="../../../.gitbook/assets/image (19) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## セットアップ

1. **Project Settings**をクリック

<figure><img src="../../../.gitbook/assets/image (30) (1).png" alt=""><figcaption></figcaption></figure>

2. **Project URL & API Key**を取得

<figure><img src="../../../.gitbook/assets/image (2) (3).png" alt=""><figcaption></figcaption></figure>

3. 各詳細（_API Key、URL、テーブル名、クエリ名_）を**Supabase**ノードにコピー＆ペースト

<figure><img src="../../../.gitbook/assets/image (85).png" alt="" width="331"><figcaption></figcaption></figure>

4. **Document**は[**Document Loader**](../document-loaders/)カテゴリの任意のノードと接続可能
5. **エンベッディング**は[**Embeddings**](../embeddings/)カテゴリの任意のノードと接続可能

## フィルタリング

メタデータキー`{source}`の下に一意の値を指定して、異なるドキュメントをアップサートしたとします。

<figure><img src="../../../.gitbook/assets/Untitled.png" alt=""><figcaption></figcaption></figure>

メタデータフィルタリングを使用して特定のメタデータをクエリできます:

**UI**

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (1) (2) (1).png" alt="" width="232"><figcaption></figcaption></figure>

**API**

```json
"overrideConfig": {
    "supabaseMetadataFilter": {
        "source": "henry"
    }
}
```

## リソース

* [LangChain JS Supabase](https://js.langchain.com/docs/modules/indexes/vector_stores/integrations/supabase)
* [Supabaseブログ投稿](https://supabase.com/blog/openai-embeddings-postgres-vector)
* [メタデータフィルタリング](https://js.langchain.com/docs/integrations/vectorstores/supabase#metadata-filtering)

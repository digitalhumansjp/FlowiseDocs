---
description: 構造化データのクエリ方法を学ぶ
---

# SQL QnA

***

[Web Scrape QnA](web-scrape-qna.md)や[Multiple Documents QnA](multiple-documents-qna.md)の例とは異なり、構造化データのクエリにはベクトルデータベースは必要ありません。高レベルでは、以下のステップで実現できます:

1. LLMに以下を提供:
   * SQLデータベーススキーマの概要
   * サンプルの行データ
2. few-shotプロンプティングでSQLクエリを返す
3. [If Else](../integrations/utilities/if-else.md)ノードを使用してSQLクエリを検証
4. SQLクエリを実行してレスポンスを取得するカスタム関数を作成
5. 実行されたSQLレスポンスから自然な応答を返す

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

この例では、SingleStoreに保存されているSQLデータベースと対話できるQnAチャットボットを作成します

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

## TL;DR

チャットフローテンプレートはこちらで見つけることができます:

{% file src="../.gitbook/assets/SQL Chatflow.json" %}

## 1. SQLデータベーススキーマ + サンプル行

カスタムJS関数ノードを使用してSingleStoreに接続し、データベーススキーマと上位3行を取得します。

[研究論文](https://arxiv.org/abs/2204.00498)によると、以下のような形式でプロンプトを生成することが推奨されています:

```
CREATE TABLE samples (firstName varchar NOT NULL, lastName varchar)
SELECT * FROM samples LIMIT 3
firstName lastName
Stephen Tyler
Jack McGinnis
Steven Repici
```

<figure><img src="../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>完全なJavaScriptコード</summary>

```javascript
const HOST = 'singlestore-host.com';
const USER = 'admin';
const PASSWORD = 'mypassword';
const DATABASE = 'mydb';
const TABLE = 'samples';
const mysql = require('mysql2/promise');

let sqlSchemaPrompt;

function getSQLPrompt() {
  return new Promise(async (resolve, reject) => {
    try {
      const singleStoreConnection = mysql.createPool({
        host: HOST,
        user: USER,
        password: PASSWORD,
        database: DATABASE,
      });

      // スキーマ情報を取得
      const [schemaInfo] = await singleStoreConnection.execute(
        `SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE table_name = "${TABLE}"`
      );

      const createColumns = [];
      const columnNames = [];

      for (const schemaData of schemaInfo) {
        columnNames.push(`${schemaData['COLUMN_NAME']}`);
        createColumns.push(`${schemaData['COLUMN_NAME']} ${schemaData['COLUMN_TYPE']} ${schemaData['IS_NULLABLE'] === 'NO' ? 'NOT NULL' : ''}`);
      }

      const sqlCreateTableQuery = `CREATE TABLE samples (${createColumns.join(', ')})`;
      const sqlSelectTableQuery = `SELECT * FROM samples LIMIT 3`;

      // 最初の3行を取得
      const [rows] = await singleStoreConnection.execute(
          sqlSelectTableQuery,
      );

      const allValues = [];
      for (const row of rows) {
          const rowValues = [];
          for (const colName in row) {
              rowValues.push(row[colName]);
          }
          allValues.push(rowValues.join(' '));
      }

      sqlSchemaPrompt = sqlCreateTableQuery + '\n' + sqlSelectTableQuery + '\n' + columnNames.join(' ') + '\n' + allValues.join('\n');

      resolve();
    } catch (e) {
      console.error(e);
      return reject(e);
    }
  });
}

async function main() {
    await getSQLPrompt();
}

await main();

return sqlSchemaPrompt;
```

</details>

`HOST`、`USER`、`PASSWORD`の取得方法については、この[ガイド](broken-reference/)で詳しく説明されています。完了したら、実行をクリックします:

<figure><img src="../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

正しい形式が生成されたことが確認できました。次のステップでは、これをプロンプトテンプレートに組み込みます。

## 2. few-shotプロンプティングによるSQLクエリの返却

新しいChat Model + Prompt Template + LLMChainを作成します

<figure><img src="../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

Prompt Templateに以下のプロンプトを指定します:

```
提供されたSQLテーブルスキーマと以下の質問に基づいて、ユーザーの質問に答えるSQLのSELECT ALLクエリを返してください。例: SELECT * FROM table WHERE id = '1'.
------------
SCHEMA: {schema}
------------
QUESTION: {question}
------------
SQL QUERY:
```

{schema}と{question}の2つの変数を使用しているため、**Format Prompt Values**でそれらの値を指定します:

<figure><img src="../.gitbook/assets/image (122).png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
LLMの学習をより良くするために、プロンプトにより多くの例（few-shotプロンプティング）を提供することができます。または[方言固有のプロンプティング](https://js.langchain.com/docs/use_cases/sql/prompting#dialect-specific-prompting)を参照してください。
{% endhint %}

## 3. [If Else](../integrations/utilities/if-else.md)ノードを使用したSQLクエリの検証

SQLクエリが無効な場合があり、無効なSQLクエリを実行するためにリソースを無駄にしたくありません。例えば、ユーザーがSQLデータベースと無関係な一般的な質問をしている場合などです。`If Else`ノードを使用して異なるパスにルーティングすることができます。

例えば、LLMが提供したSQLクエリにSELECTとWHEREが含まれているかどうかの基本的なチェックを実行できます。

{% tabs %}
{% tab title="If Function" %}
```javascript
const sqlQuery = $sqlQuery.trim();

const regex = /SELECT\s.*?(?:\n|$)/gi;

// SQLパートの抽出
const matches = sqlQuery.match(regex);
const cleanSql = matches ? matches[0].trim() : "";

if (cleanSql.includes("SELECT") && cleanSql.includes("WHERE")) {
    return cleanSql;
}
```
{% endtab %}

{% tab title="Else Function" %}
```javascript
return $sqlQuery;
```
{% endtab %}
{% endtabs %}

<figure><img src="../.gitbook/assets/image (119).png" alt="" width="327"><figcaption></figcaption></figure>

Else Functionでは、LLMにユーザークエリに回答できないことを伝えるPrompt Template + LLMChainにルーティングします:

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

## 4. SQLクエリを実行してレスポンスを取得するカスタム関数

有効なSQLクエリの場合、クエリを実行する必要があります。**If Else**ノードの_**True**_出力を**Custom JS Function**ノードに接続します:

<figure><img src="../.gitbook/assets/image (123).png" alt="" width="563"><figcaption></figcaption></figure>

<details>

<summary>完全なJavaScriptコード</summary>

```javascript
const HOST = 'singlestore-host.com';
const USER = 'admin';
const PASSWORD = 'mypassword';
const DATABASE = 'mydb';
const TABLE = 'samples';
const mysql = require('mysql2/promise');

let result;

function getSQLResult() {
  return new Promise(async (resolve, reject) => {
    try {
      const singleStoreConnection = mysql.createPool({
        host: HOST,
        user: USER,
        password: PASSWORD,
        database: DATABASE,
      });

      const [rows] = await singleStoreConnection.execute(
        $sqlQuery
      );

      result = JSON.stringify(rows)

      resolve();
    } catch (e) {
      console.error(e);
      return reject(e);
    }
  });
}

async function main() {
    await getSQLResult();
}

await main();

return result;
```

</details>

## 5. 実行されたSQLレスポンスから自然な応答を返す

新しいChat Model + Prompt Template + LLMChainを作成します

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

Prompt Templateに以下のプロンプトを記述します:

```
質問とSQLレスポンスに基づいて、できるだけ詳細な自然言語での応答を作成してください:
------------
QUESTION: {question}
------------
SQL RESPONSE: {sqlResponse}
------------
NATURAL LANGUAGE RESPONSE:
```

**Format Prompt Values**で変数を指定します:

<figure><img src="../.gitbook/assets/image (125).png" alt="" width="563"><figcaption></figcaption></figure>

これで完了です！SQLチャットボットのテスト準備が整いました！

## クエリ

まず、データベースに関連する質問をしてみましょう。

<figure><img src="../.gitbook/assets/image (128).png" alt="" width="434"><figcaption></figcaption></figure>

ログを見ると、最初のLLMChainがSQLクエリを生成できていることがわかります:

**入力:**

{% code overflow="wrap" %}
```
Based on the provided SQL table schema and question below, return a SQL SELECT ALL query that would answer the user's question. For example: SELECT * FROM table WHERE id = '1'.\n------------\nSCHEMA: CREATE TABLE samples (id bigint(20) NOT NULL, firstName varchar(300) NOT NULL, lastName varchar(300) NOT NULL, userAddress varchar(300) NOT NULL, userState varchar(300) NOT NULL, userCode varchar(300) NOT NULL, userPostal varchar(300) NOT NULL, createdate timestamp(6) NOT NULL)\nSELECT * FROM samples LIMIT 3\nid firstName lastName userAddress userState userCode userPostal createdate\n1125899906842627 Steven Repici 14 Kingston St. Oregon NJ 5578 Thu Dec 14 2023 13:06:17 GMT+0800 (Singapore Standard Time)\n1125899906842625 John Doe 120 jefferson st. Riverside NJ 8075 Thu Dec 14 2023 13:04:32 GMT+0800 (Singapore Standard Time)\n1125899906842629 Bert Jet 9th, at Terrace plc Desert City CO 8576 Thu Dec 14 2023 13:07:11 GMT+0800 (Singapore Standard Time)\n------------\nQUESTION: what is the address of John\n------------\nSQL QUERY:
```
{% endcode %}

**出力**

<pre class="language-sql"><code class="lang-sql"><strong>SELECT userAddress FROM samples WHERE firstName = 'John'
</strong></code></pre>

SQLクエリを実行した後、結果は2番目のLLMChainに渡されます:

**入力**

{% code overflow="wrap" %}
```
Based on the question, and SQL response, write a natural language response, be details as possible:\n------------\nQUESTION: what is the address of John\n------------\nSQL RESPONSE: [{\"userAddress\":\"120 jefferson st.\"}]\n------------\nNATURAL LANGUAGE RESPONSE:
```
{% endcode %}

**出力**

```
The address of John is 120 Jefferson St.
```

次に、SQLデータベースと無関係な質問をすると、Elseルートが実行されます。

<figure><img src="../.gitbook/assets/image (132).png" alt="" width="428"><figcaption></figcaption></figure>

最初のLLMChainでは、以下のようなSQLクエリが生成されます:

```sql
SELECT * FROM samples LIMIT 3
```

しかし、`SELECT`と`WHERE`の両方を含んでいないため`If Else`チェックに失敗し、以下のプロンプトを持つElseルートに入ります:

```
Politely say "I'm not able to answer query"
```

最終的な出力は:

```
I apologize, but I'm not able to answer your query at the moment.
```

## まとめ

この例では、データベースと対話でき、かつデータベースと無関係な質問も処理できるSQLチャットボットを作成することに成功しました。会話履歴を提供するためのメモリを追加することで、さらに改善することができます。

以下のチャットフローを参照できます:

{% file src="../.gitbook/assets/SQL Chatflow (1).json" %}

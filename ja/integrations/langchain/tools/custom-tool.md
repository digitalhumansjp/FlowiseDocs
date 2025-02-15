# カスタムツール

カスタムツールの使用方法をご覧ください

{% embed url="https://youtu.be/HSp9LkkTVY0" %}

## 問題

関数は通常、構造化された入力データを受け取ります。例えば、LLMにAirtable Create Record [API](https://airtable.com/developers/web/api/create-records)を呼び出させたい場合、ボディパラメータは特定の方法で構造化される必要があります。例えば:

```json
"records": [
  {
    "fields": {
      "Address": "住所",
      "Name": "名前",
      "Visited": true
    }
  }
]
```

理想的には、LLMが以下のような適切な構造化データを返すことが望ましいです:

```json
{
  "Address": "住所",
  "Name": "名前",
  "Visited": true
}
```

これにより、APIに必要なボディにパースして値を抽出することができます。しかし、LLMに正確なパターンを出力するよう指示することは困難です。

新しい[OpenAI Function Calling](https://openai.com/blog/function-calling-and-other-api-updates)モデルにより、これが可能になりました。`gpt-4-0613`と`gpt-3.5-turbo-0613`は構造化データを返すように特別に訓練されています。モデルは関数を呼び出すための引数を含むJSONオブジェクトを出力することを賢く選択します。

## チュートリアル

**目標**: エージェントが自動的に株価の動きを取得し、関連する株式ニュースを取得し、Airtableに新しいレコードを追加します。

始めましょう[🚀](https://emojipedia.org/rocket/)

### ツールの作成

目標を達成するために3つのツールが必要です:

* 株価の動きを取得
* 株式ニュースを取得
* Airtableレコードを追加

#### 株価の動きを取得

以下の詳細で新しいツールを作成します(必要に応じて変更可能):

* 名前: get_stock_movers
* 説明: 株価/出来高の動きが最も大きい銘柄(アクティブ、値上がり、値下がりなど)を取得します

説明は重要な要素です。ChatGPTはこれを参考にしてこのツールをいつ使用するかを判断します。

<figure><img src="../../../.gitbook/assets/image (6) (3).png" alt=""><figcaption></figcaption></figure>

* JavaScript関数: [Morning Star](https://rapidapi.com/apidojo/api/morning-star)の`/market/v2/get-movers` APIを使用してデータを取得します。まだSubscribe to Testをクリックしていない場合は、まずそれを行い、コードをコピーしてJavaScript関数に貼り付けます。
  * ライブラリをインポートするために、先頭に`const fetch = require('node-fetch');`を追加します。任意の組み込みNodeJS[モジュール](https://www.w3schools.com/nodejs/ref_modules.asp)と[外部ライブラリ](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289)をインポートできます。
  * 最後に`result`を返します。

<figure><img src="../../../.gitbook/assets/Untitled (4) (1).png" alt=""><figcaption></figcaption></figure>

最終的なコードは以下のようになります:

```javascript
const fetch = require('node-fetch');
const url = 'https://morning-star.p.rapidapi.com/market/v2/get-movers';
const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': 'APIキーに置き換えてください',
		'X-RapidAPI-Host': 'morning-star.p.rapidapi.com'
	}
};

try {
	const response = await fetch(url, options);
	const result = await response.text();
	console.log(result);
	return result;
} catch (error) {
	console.error(error);
	return '';
}
```

これで保存できます。

#### 株式ニュースを取得

以下の詳細で新しいツールを作成します(必要に応じて変更可能):

* 名前: get_stock_news
* 説明: 株式の最新ニュースを取得
* 入力スキーマ:
  * プロパティ: performanceId
  * タイプ: string
  * 説明: APIでperformanceIDと呼ばれる株式のID
  * 必須: true

入力スキーマは、LLMが返すべきJSONオブジェクトを指定します。この場合、以下のようなJSONオブジェクトを期待しています:

<pre class="language-json"><code class="lang-json"><strong>{ "performanceId": "銘柄コード" }
</strong></code></pre>

<figure><img src="../../../.gitbook/assets/image (4) (2).png" alt=""><figcaption></figcaption></figure>

* JavaScript関数: [Morning Star](https://rapidapi.com/apidojo/api/morning-star)の`/news/list` APIを使用してデータを取得します。まだSubscribe to Testをクリックしていない場合は、まずそれを行い、コードをコピーしてJavaScript関数に貼り付けます。
  * ライブラリをインポートするために、先頭に`const fetch = require('node-fetch');`を追加します。任意の組み込みNodeJS[モジュール](https://www.w3schools.com/nodejs/ref_modules.asp)と[外部ライブラリ](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289)をインポートできます。
  * 最後に`result`を返します。
* 次に、URLクエリパラメータのperformanceIdのハードコードされた値`0P0000OQN8`を入力スキーマで指定したプロパティ変数`$performanceId`に置き換えます。
* JavaScript関数内で入力スキーマで指定したプロパティを変数として使用する場合は、変数名の前に`$`を付けます。

<figure><img src="../../../.gitbook/assets/Untitled (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

最終的なコード:

```javascript
const fetch = require('node-fetch');
const url = 'https://morning-star.p.rapidapi.com/news/list?performanceId=' + $performanceId;
const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': 'APIキーに置き換えてください',
		'X-RapidAPI-Host': 'morning-star.p.rapidapi.com'
	}
};

try {
	const response = await fetch(url, options);
	const result = await response.text();
	console.log(result);
	return result;
} catch (error) {
	console.error(error);
	return '';
}
```

これで保存できます。

#### Airtableレコードを追加

以下の詳細で新しいツールを作成します(必要に応じて変更可能):

* 名前: add_airtable
* 説明: 株式、ニュースサマリー、価格変動をAirtableに追加
* 入力スキーマ:
  * プロパティ: stock
  * タイプ: string
  * 説明: 株式の銘柄コード
  * 必須: true
  * プロパティ: move
  * タイプ: string
  * 説明: 価格変動(%)
  * 必須: true
  * プロパティ: news_summary
  * タイプ: string
  * 説明: 株式のニュースサマリー
  * 必須: true

ChatGPTは以下のようなJSONオブジェクトを返します:

```json
{ "stock": "銘柄コード", "move": "20%", "news_summary": "サマリー" }
```

<figure><img src="../../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

* JavaScript関数: [Airtable Create Record API](https://airtable.com/developers/web/api/create-records)を使用して既存のテーブルに新しいレコードを作成します。tableIdとbaseIdは[ここ](https://www.highviewapps.com/kb/where-can-i-find-the-airtable-base-id-and-table-id/)から見つけることができます。また、パーソナルアクセストークンの作成も必要です。作成方法は[ここ](https://www.highviewapps.com/kb/how-do-i-create-an-airtable-personal-access-token/)を参照してください。

最終的なコードは以下のようになります。`$stock`、`$move`、`$news_summary`を変数として渡す方法に注目してください:

```javascript
const fetch = require('node-fetch');
const baseId = 'ベースID';
const tableId = 'テーブルID';
const token = 'トークン';

const body = {
	"records": [
		{
			"fields": {
				"stock": $stock,
				"move": $move,
				"news_summary": $news_summary,
			}
		}
	]
};

const options = {
	method: 'POST',
	headers: {
		'Authorization': `Bearer ${token}`,
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

const url = `https://api.airtable.com/v0/${baseId}/${tableId}`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```

これで保存できます。

3つのツールが作成されているはずです:

<figure><img src="../../../.gitbook/assets/image (3) (3) (1).png" alt=""><figcaption></figcaption></figure>

### チャットフローの作成

マーケットプレイスから**OpenAI Function** **Agent**テンプレートを使用し、ツールを**カスタムツール**に置き換えることができます。作成したツールを選択してください。

注意: OpenAI Function Agentは現在0613モデルのみをサポートしています。

<figure><img src="../../../.gitbook/assets/image (15) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

チャットフローを保存してテストを開始します。最初に、以下のような質問を試してみましょう:

_<mark style="color:blue;">今日、最も株価の変動が大きかった銘柄は何ですか？</mark>_

_<mark style="color:orange;">今日、最も株価の変動が大きかった銘柄はOverstock.com (OSTK)で、価格変動は17.47%でした。</mark>_

次に、その特定の銘柄に関するニュースを取得する質問をフォローアップできます:

_<mark style="color:blue;">この株価変動の原因となった最新のニュースは何ですか？</mark>_

_<mark style="color:orange;">Overstock.com (OSTK)の株価変動の原因となった可能性のある最新ニュースは以下の通りです:</mark>_

1. _<mark style="color:orange;">タイトル: "Overstockの株価、Bed Bath & Beyond資産の2150万ドルの入札成功で急上昇" ソース: MarketWatch 公開日: 2023年6月22日 サマリー: Overstock.comの株価は、Bed Bath & Beyond資産の2150万ドルの入札に成功した後、大幅に上昇しました。</mark>_
2. _<mark style="color:orange;">タイトル: "Meta Platforms、Overstock.com、Walmart、Home Depot、United Parcel Serviceのオプションや株式取引を検討していますか？" ソース: PR Newswire 公開日: 2023年6月22日 サマリー: この記事では、投資家が検討する可能性のあるOverstock.comを含む潜在的な取引オプションと株式について説明しています。</mark>_

_<mark style="color:orange;">これらのニュース記事は情報提供のみを目的としており、株価変動の唯一の理由ではない可能性があることにご注意ください。投資判断を行う前に、徹底的な調査と分析を行うことを常にお勧めします。</mark>_

最後に、ChatGPTにAirtableに新しいレコードを追加するよう依頼できます:

_<mark style="color:blue;">銘柄コード、価格変動、ニュースサマリーをAirtableにレコードとして追加できますか？</mark>_

_<mark style="color:orange;">以下の詳細でAirtableにレコードを追加しました:</mark>_

_<mark style="color:orange;">銘柄コード: OSTK 価格変動: 17.47% ニュースサマリー: Overstock.comの株価は、Bed Bath & Beyond資産の2150万ドルの入札に成功した後、大幅に上昇しました。</mark>_

[🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)**完成です！** これがカスタムツールを作成してOpenAI Function Agentで使用する方法です！

## 追加情報

### 関数にセッションIDを渡す

デフォルトでは、カスタムツールの関数は以下のフロー設定にアクセスできます:

```json5
$flow.sessionId
$flow.chatId
$flow.chatflowId
$flow.input
```

以下はセッションIDをDiscordウェブフックに送信する例です:

{% tabs %}
{% tab title="Javascript" %}
```javascript
const fetch = require('node-fetch');
const webhookUrl = "https://discord.com/api/webhooks/1124783587267";
const content = $content; // 入力スキーマから取得
const sessionId = $flow.sessionId;

const body = {
	"content": `${mycontent} and the sessionid is ${sessionId}`
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

const url = `${webhookUrl}?wait=true`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```
{% endtab %}
{% endtabs %}

### 関数に変数を渡す

場合によっては、カスタムツール関数に変数を渡したいことがあります。

例えば、カスタムツールを使用するチャットボットを作成する場合、カスタムツールがHTTP POSTコールを実行し、認証されたリクエストを成功させるためにAPIキーが必要な場合があります。これを変数として渡すことができます。

デフォルトでは、カスタムツールの関数は以下の変数にアクセスできます:

```
$vars.<変数名>
```

FlowiseでAPIと埋め込みを使用して変数を渡す例:

{% tabs %}
{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflow-id>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "こんにちは、お元気ですか？",
    "overrideConfig": {
        "vars": {
            "apiKey": "abc"
        }
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}

{% tab title="埋め込み" %}
```html
<script type="module">
    import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
    Chatbot.init({
        chatflowid: 'chatflow-id',
        apiHost: 'http://localhost:3000',
        chatflowConfig: {
          vars: {
            apiKey: 'def'
          }
        }
    });
</script>
```
{% endtab %}
{% endtabs %}

カスタムツールで変数を受け取る例:

{% tabs %}
{% tab title="Javascript" %}
```javascript
const fetch = require('node-fetch');
const webhookUrl = "https://discord.com/api/webhooks/1124783587267";
const content = $content; // 入力スキーマから取得
const sessionId = $flow.sessionId;
const apiKey = $vars.apiKey;

const body = {
	"content": `${mycontent} and the sessionid is ${sessionId}`
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json',
		'Authorization': `Bearer ${apiKey}`
	},
	body: JSON.stringify(body)
};

const url = `${webhookUrl}?wait=true`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```
{% endtab %}
{% endtabs %}

### カスタムツールのオーバーライド

以下のパラメータをオーバーライドできます

| パラメータ       | 説明             |
| ---------------- | ---------------- |
| customToolName   | ツール名         |
| customToolDesc   | ツールの説明     |
| customToolSchema | ツールのスキーマ |
| customToolFunc   | ツールの関数     |

カスタムツールのパラメータをオーバーライドするAPIコールの例:

{% tabs %}
{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflow-id>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "こんにちは、お元気ですか？",
    "overrideConfig": {
        "customToolName": "example_tool",
        "customToolSchema": "z.object({title: z.string()})"
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### 外部依存関係のインポート

任意の組み込みNodeJS[モジュール](https://www.w3schools.com/nodejs/ref_modules.asp)とサポートされている[外部ライブラリ](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289)を関数にインポートできます。

1. サポートされていないライブラリをインポートするには、`packages/components`フォルダの`package.json`に新しいnpmパッケージを簡単に追加できます。

```bash
cd Flowise && cd packages && cd components
pnpm add <あなたのライブラリ>
cd .. && cd ..
pnpm install
pnpm build
```

2. 次に、インポートしたライブラリを`TOOL_FUNCTION_EXTERNAL_DEP`環境変数に追加します。詳細は[#builtin-and-external-dependencies](../../../configuration/environment-variables.md#builtin-and-external-dependencies "mention")を参照してください。
3. アプリを起動します

```bash
pnpm start
```

4. これで、**JavaScript関数**で新しく追加したライブラリを以下のように使用できます:

```javascript
const axios = require('axios')
```

追加の依存関係を追加してライブラリをインポートする方法をご覧ください

{% embed url="https://youtu.be/0H1rrisc0ok" %}

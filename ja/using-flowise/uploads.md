---
description: 画像、音声、その他のファイルのアップロード方法について学ぶ
---

# アップロード

Flowiseでは、チャットから画像、音声、その他のファイルをアップロードできます。このセクションでは、これらの機能を有効にして使用する方法について説明します。

## 画像

特定のチャットモデルでは画像を入力できます。モデルが画像入力をサポートしているかどうかは、必ずLLMの公式ドキュメントを参照してください。

* [ChatOpenAI](../integrations/llamaindex/chat-models/chatopenai.md)
* [AzureChatOpenAI](../integrations/llamaindex/chat-models/azurechatopenai.md)
* [ChatAnthropic](../integrations/langchain/chat-models/chatanthropic.md)
* [AWSChatBedrock](../integrations/langchain/chat-models/aws-chatbedrock.md)
* [ChatGoogleGenerativeAI](../integrations/langchain/chat-models/google-ai.md)
* [ChatOllama](../integrations/llamaindex/chat-models/chatollama.md)
* [Google Vertex AI](../integrations/langchain/llms/googlevertex-ai.md)

{% hint style="warning" %}
画像処理は、Chatflowの特定のチェーン/エージェントでのみ機能します。

[LLMChain](../integrations/langchain/chains/llm-chain.md)、[Conversation Chain](../integrations/langchain/chains/conversation-chain.md)、[ReAct Agent](../integrations/langchain/agents/react-agent-chat.md)、[Conversational Agent](../integrations/langchain/agents/conversational-agent.md)、[Tool Agent](../integrations/langchain/agents/tool-agent.md)
{% endhint %}

**画像アップロードを許可**を有効にすると、チャットインターフェースから画像をアップロードできます。

<div align="center"><figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="255"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 011714.png" alt="" width="290"><figcaption></figcaption></figure></div>

APIで画像をアップロードするには:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "question": "この画像を説明できますか?",
    "uploads": [
        {
            "data": "data:image/png;base64,iVBORw0KGgdM2uN0", # base64文字列またはURL
            "type": "file", # file | url
            "name": "Flowise.png",
            "mime": "image/png"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
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
    "question": "この画像を説明できますか?",
    "uploads": [
        {
            "data": "data:image/png;base64,iVBORw0KGgdM2uN0", //base64文字列またはURL
            "type": "file", // file | url
            "name": "Flowise.png",
            "mime": "image/png"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## 音声

Chatflowの設定で、音声認識モジュールを選択できます。サポートされている連携先は以下の通りです:

* OpenAI
* AssemblyAI
* [LocalAI](../integrations/langchain/chat-models/chatlocalai.md)

これが有効になっている場合、ユーザーは直接マイクに向かって話すことができます。音声はテキストに変換されます。

<div align="left"><figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 012538.png" alt="" width="431"><figcaption></figcaption></figure></div>

APIで音声をアップロードするには:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "uploads": [
        {
            "data": "data:audio/webm;codecs=opus;base64,GkXf", # base64文字列
            "type": "audio",
            "name": "audio.wav",
            "mime": "audio/webm"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
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
    "uploads": [
        {
            "data": "data:audio/webm;codecs=opus;base64,GkXf", // base64文字列
            "type": "audio",
            "name": "audio.wav",
            "mime": "audio/webm"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## ファイル

ファイルのアップロードには2つの方法があります:

* 検索拡張生成(RAG)ファイルアップロード
* 完全ファイルアップロード

両方のオプションが有効な場合、完全ファイルアップロードが優先されます。

### RAG ファイルアップロード

アップロードされたファイルをベクトルストアにその場でアップサートできます。ファイルアップロードを有効にするには、以下の前提条件を満たす必要があります:

* ファイルアップロードをサポートするベクトルストアをチャットフローに含める必要があります。
  * [Pinecone](../integrations/langchain/vector-stores/pinecone.md)
  * [Milvus](../integrations/langchain/vector-stores/milvus.md)
  * [Postgres](../integrations/langchain/vector-stores/postgres.md)
  * [Qdrant](../integrations/langchain/vector-stores/qdrant.md)
  * [Upstash](../integrations/langchain/vector-stores/upstash-vector.md)
* チャットフローに複数のベクトルストアがある場合、一度に1つのベクトルストアでのみファイルアップロードを有効にできます。
* ベクトルストアのドキュメント入力に少なくとも1つのドキュメントローダーノードを接続する必要があります。
* サポートされているドキュメントローダー:
  * [CSVファイル](../integrations/langchain/document-loaders/csv-file.md)
  * [Docxファイル](../integrations/langchain/document-loaders/docx-file.md)
  * [Jsonファイル](../integrations/langchain/document-loaders/json-file.md)
  * [Json Linesファイル](../integrations/langchain/document-loaders/json-lines-file.md)
  * [PDFファイル](../integrations/langchain/document-loaders/pdf-file.md)
  * [テキストファイル](../integrations/langchain/document-loaders/text-file.md)
  * [非構造化ファイル](../integrations/langchain/document-loaders/unstructured-file-loader.md)

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

チャットで1つまたは複数のファイルをアップロードできます:

<div align="left"><figure><img src="../.gitbook/assets/image (3) (1) (1) (1).png" alt="" width="380"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-08-26 170456.png" alt=""><figcaption></figcaption></figure></div>

仕組みは以下の通りです:

1. アップロードされたファイルのメタデータがchatIdで更新されます。
2. これによりファイルとchatIdが関連付けられます。
3. クエリ時には、**OR**フィルターが適用されます:

* メタデータに`flowise_chatId`が含まれ、値が現在のチャットセッションIDである
* メタデータに`flowise_chatId`が含まれない

Pineconeにアップサートされたベクトル埋め込みの例:

<figure><img src="../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

APIでこれを行うには、以下の2つのステップに従います:

1. [Vector Upsert API](api.md#vector-upsert-api)を`formData`と`chatId`で使用:

{% tabs %}
{% tab title="Python" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowid>"

# ファイルをアップロードするためにform dataを使用
form_data = {
    "files": ("state_of_the_union.txt", open("state_of_the_union.txt", "rb"))
}

body_data = {
    "chatId": "some-session-id"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
// ファイルをアップロードするためにFormDataを使用
let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("chatId", "some-session-id");

async function query(formData) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowid>",
        {
            method: "POST",
            body: formData
        }
    );
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

2. ステップ1の`chatId`と`uploads`を使用して[Prediction API](api.md#prediction)を使用:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "question": "スピーチの内容は何ですか?",
    "chatId": "ステップ1と同じセッションID",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:rag",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
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
    "question": "スピーチの内容は何ですか?",
    "chatId": "ステップ1と同じセッションID",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:rag",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}### 完全ファイルアップロード

RAGファイルアップロードでは、スプレッドシートやテーブルなどの構造化データを扱うことができず、完全なコンテキストがないため完全な要約を実行することもできません。場合によっては、特にGeminiやClaudeのような長いコンテキストウィンドウを持つモデルで、ファイルの内容全体をプロンプトに直接含めたい場合があります。[この研究論文](https://arxiv.org/html/2407.16833v1)は、RAGと長いコンテキストウィンドウを比較した多くの論文の1つです。

完全ファイルアップロードを有効にするには、**Chatflow Configuration**に移動し、**File Upload**タブを開いてスイッチをクリックします:

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

チャットで**ファイル添付**ボタンが表示され、1つまたは複数のファイルをアップロードできます。内部的には、[File Loader](../integrations/langchain/document-loaders/file-loader.md)が各ファイルを処理してテキストに変換します。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

APIでファイルをアップロードするには:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "question": "このデータは何に関するものですか?",
    "chatId": "some-session-id",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:full",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
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
    "question": "このデータは何に関するものですか?",
    "chatId": "some-session-id",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:full",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

例からわかるように、アップロードにはbase64文字列が必要です。ファイルのbase64文字列を取得するには、[Create Attachments API](../api-reference/attachments.md)を使用してください。

### 完全アップロードとRAGアップロードの違い

完全アップロードとRAG（検索拡張生成）ファイルアップロードは、それぞれ異なる目的で使用されます。

* **完全ファイルアップロード**: このメソッドはファイル全体を文字列に解析し、LLM（大規模言語モデル）に送信します。文書の要約や重要な情報の抽出に適しています。ただし、非常に大きなファイルの場合、トークン制限により、モデルが不正確な結果や「幻覚」を生成する可能性があります。
* **RAGファイルアップロード**: LLMに全テキストを送信しないことでトークンコストを削減したい場合に推奨されます。このアプローチは文書に関するQ&Aタスクに適していますが、文書全体のコンテキストがないため要約には適していません。アップサート処理のため、時間がかかる場合があります。

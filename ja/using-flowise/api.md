---
description: よく使用されるAPI（予測、ベクトルアップサート）の詳細について学ぶ
---

# API

公開APIの完全なリストについては、[APIリファレンス](../api-reference/)を参照してください。

## 予測

<div data-full-width="false"><figure><img src="../.gitbook/assets/image (16) (1) (1) (1).png" alt=""><figcaption></figcaption></figure></div>

{% swagger src="../.gitbook/assets/swagger (1) (1) (1).yml" path="/prediction/{id}" method="post" %}
[swagger (1) (1) (1).yml](<../.gitbook/assets/swagger (1) (1) (1).yml>)
{% endswagger %}

### Python/TSライブラリの使用

Flowiseは2つのライブラリを提供しています:

* [Python](https://pypi.org/project/flowise/): `pip install flowise`
* [Typescript](https://www.npmjs.com/package/flowise-sdk): `npm install flowise-sdk`

{% tabs %}
{% tab title="Python SDK" %}
```python
from flowise import Flowise, PredictionData

def test_non_streaming():
    client = Flowise()

    # ストリーミングなしの予測をテスト
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="What is the capital of France?",
            streaming=False
        )
    )

    # レスポンスを処理して表示
    for response in completion:
        print("Non-streaming response:", response)

def test_streaming():
    client = Flowise()

    # ストリーミング予測をテスト
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="Tell me a joke!",
            streaming=True
        )
    )

    # ストリーミングされた各チャンクを処理して表示
    print("Streaming response:")
    for chunk in completion:
        print(chunk)


if __name__ == "__main__":
    # ストリーミングなしのテストを実行
    test_non_streaming()

    # ストリーミングのテストを実行
    test_streaming()
```
{% endtab %}

{% tab title="Typescript SDK" %}
```javascript
import { FlowiseClient } from 'flowise-sdk'

async function test_streaming() {
  const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });

  try {
    // ストリーミング予測の場合
    const prediction = await client.createPrediction({
      chatflowId: 'fe1145fa-1b2b-45b7-b2ba-bcc5aaeb5ffd',
      question: 'What is the revenue of Apple?',
      streaming: true,
    });

    for await (const chunk of prediction) {
        console.log(chunk);
    }

  } catch (error) {
    console.error('Error:', error);
  }
}

async function test_non_streaming() {
    const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });

    try {
      // ストリーミング予測の場合
      const prediction = await client.createPrediction({
        chatflowId: 'fe1145fa-1b2b-45b7-b2ba-bcc5aaeb5ffd',
        question: 'What is the revenue of Apple?',
      });

      console.log(prediction);

    } catch (error) {
      console.error('Error:', error);
    }
}

// ストリーミングなしのテストを実行
test_non_streaming()

// ストリーミングのテストを実行
test_streaming()
```
{% endtab %}
{% endtabs %}

### 設定の上書き

チャットフローの既存の入力設定を**overrideConfig**プロパティで上書きします。

セキュリティ上の理由から、設定の上書きはデフォルトで無効になっています。ユーザーは**チャットフロー設定**-> **セキュリティ**タブから、これを有効にする必要があります。その後、上書き可能なプロパティを選択します。

<figure><img src="../.gitbook/assets/image (188).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123",
        "returnSourceDocuments": true
    }
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123",
        "returnSourceDocuments": true
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### 履歴

LLMにコンテキストを与えるために、履歴メッセージを前置きすることができます。例えば、LLMにユーザーの名前を覚えさせたい場合:

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "question": "Hey, how are you?",
    "history": [
        {
            "role": "apiMessage",
            "content": "Hello how can I help?"
        },
        {
            "role": "userMessage",
            "content": "Hi my name is Brian"
        },
        {
            "role": "apiMessage",
            "content": "Hi Brian, how can I help?"
        },
    ]
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
    "question": "Hey, how are you?",
    "history": [
        {
            "role": "apiMessage",
            "content": "Hello how can I help?"
        },
        {
            "role": "userMessage",
            "content": "Hi my name is Brian"
        },
        {
            "role": "apiMessage",
            "content": "Hi Brian, how can I help?"
        },
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### メモリの永続化

会話の状態を永続化するために`sessionId`を渡すことができます。これにより、その後のAPI呼び出しはすべて以前の会話のコンテキストを持つことになります。指定しない場合は、毎回新しいセッションが生成されます。

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123"
    }
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123"
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### 変数

フロー内のノードで使用する変数をAPIに渡します。詳細は[変数](api.md#variables)を参照してください。

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "vars": {
            "foo": "bar"
        }
    }
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
    "question": "Hey, how are you?",
    "overrideConfig": {
        "vars": {
            "foo": "bar"
        }
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### 画像のアップロード

**画像のアップロードを許可**が有効な場合、チャットインターフェースから画像をアップロードできます。

<div align="left" data-full-width="false"><figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="255"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 011714.png" alt="" width="290"><figcaption></figcaption></figure></div>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "question": "Can you describe the image?",
    "uploads": [
        {
            "data": 'data:image/png;base64,iVBORw0KGgdM2uN0', # base64文字列またはURL
            "type": 'file', # file | url
            "name": 'Flowise.png',
            "mime": 'image/png'
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
    "question": "Can you describe the image?",
    "uploads": [
        {
            "data": 'data:image/png;base64,iVBORw0KGgdM2uN0', //base64文字列またはURL
            "type": 'file', //file | url
            "name": 'Flowise.png',
            "mime": 'image/png'
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### 音声のテキスト変換

**音声のテキスト変換**が有効な場合、ユーザーはマイクに直接話しかけることができ、音声がテキストに変換されます。

<div align="left"><figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 012538.png" alt="" width="431"><figcaption></figcaption></figure></div>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "uploads": [
        {
            "data": 'data:audio/webm;codecs=opus;base64,GkXf', #base64文字列
            "type": 'audio',
            "name": 'audio.wav',
            "mime": 'audio/webm'
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
            "data": 'data:audio/webm;codecs=opus;base64,GkXf', //base64文字列
            "type": 'audio',
            "name": 'audio.wav',
            "mime": 'audio/webm'
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## ベクトルアップサートAPI

{% swagger src="../.gitbook/assets/swagger (1) (1) (1).yml" path="/vector/upsert/{id}" method="post" %}
[swagger (1) (1) (1).yml](<../.gitbook/assets/swagger (1) (1) (1).yml>)
{% endswagger %}

### ファイルアップロード機能付きドキュメントローダー

Flowiseのいくつかのドキュメントローダーでは、ユーザーがファイルをアップロードできます:

* [CSVファイル](../integrations/langchain/document-loaders/csv-file.md)
* [Docxファイル](../integrations/langchain/document-loaders/docx-file.md)
* [Jsonファイル](../integrations/langchain/document-loaders/json-file.md)
* [JsonLinesファイル](../integrations/langchain/document-loaders/json-lines-file.md)
* [PDFファイル](../integrations/langchain/document-loaders/pdf-file.md)
* [テキストファイル](../integrations/langchain/document-loaders/text-file.md)
* [非構造化ファイル](../integrations/langchain/document-loaders/unstructured-file-loader.md)

<div data-full-width="false"><figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure></div>

フローにファイルアップロード機能を持つ[ドキュメントローダー](../integrations/langchain/document-loaders/)が含まれている場合、APIは少し異なります。本文をJSONとして渡す代わりに、**フォームデータ**が使用されます。これによりAPIにファイルを送信することができます。

{% hint style="info" %}
送信するファイルタイプがドキュメントローダーで期待されるファイルタイプと互換性があることを確認してください。例えば、PDFファイルローダーを使用している場合は、**.pdf**ファイルのみを送信する必要があります。

異なるファイルタイプごとに別々のローダーを持つことを避けるために、[ファイルローダー](../integrations/langchain/document-loaders/file-loader.md)の使用をお勧めします。
{% endhint %}

{% tabs %}
{% tab title="Python API" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowId>"

# ファイルをアップロードするためにフォームデータを使用
form_data = {
    "files": ('state_of_the_union.txt', open('state_of_the_union.txt', 'rb'))
}

body_data = {
    "returnSourceDocuments": True
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
// ファイルをアップロードするためにFormDataを使用
let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("returnSourceDocuments", true);

async function query(formData) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowId>",
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

### アップロード機能のないドキュメントローダー

ファイルアップロード機能のない他の[ドキュメントローダー](../integrations/langchain/document-loaders/)ノードの場合、APIボディは[予測API](api.md#prediction-api)と同様に**JSON**形式です。

{% tabs %}
{% tab title="Python API" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    print(response)
    return response.json()

output = query({
    "overrideConfig": { # オプション
        "returnSourceDocuments": true
    }
})
print(output)
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowId>",
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
    "overrideConfig": { // オプション
        "returnSourceDocuments": true
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## ドキュメントアップサート/リフレッシュAPI

APIの使用方法の詳細については、[ドキュメントストア](document-stores.md#id-10.-api)セクションを参照してください。

{% swagger src="../.gitbook/assets/swagger (2) (1).yml" path="/document-store/upsert/{id}" method="post" %}
[swagger (2) (1).yml](<../.gitbook/assets/swagger (2) (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (2) (1).yml" path="/document-store/refresh/{id}" method="post" %}
[swagger (2) (1).yml](<../.gitbook/assets/swagger (2) (1).yml>)
{% endswagger %}

## ビデオチュートリアル

これらのビデオチュートリアルでは、Flowise APIを実装するための主なユースケースを説明しています。

{% embed url="https://youtu.be/9R5zo3IVkqU?si=y1v_aCQLE_70WBnA" %}

{% embed url="https://youtu.be/LhN560DhlzU" %}

{% embed url="https://youtu.be/kOwmPe8aLAA" %}

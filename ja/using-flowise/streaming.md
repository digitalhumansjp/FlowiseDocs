---
description: Flowiseのストリーミングの仕組みを学ぶ
---

# ストリーミング

予測時にストリーミングが設定されている場合、トークンは利用可能になり次第、データのみの[サーバー送信イベント](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events#Event_stream_format)として送信されます。

### Python/TSライブラリの使用

Flowiseは2つのライブラリを提供しています:

* [Python](https://pypi.org/project/flowise/): `pip install flowise`
* [Typescript](https://www.npmjs.com/package/flowise-sdk): `npm install flowise-sdk`

{% tabs %}
{% tab title="Python" %}
```python
from flowise import Flowise, PredictionData

def test_streaming():
    client = Flowise()

    # ストリーミング予測のテスト
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
        # {event: "token", data: "hello"}
        print(chunk)


if __name__ == "__main__":
    test_streaming()
```
{% endtab %}

{% tab title="Typescript" %}
```javascript
import { FlowiseClient } from 'flowise-sdk'

async function test_streaming() {
  const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });

  try {
    // ストリーミング予測の場合
    const prediction = await client.createPrediction({
      chatflowId: '<chatflow-id>',
      question: 'What is the capital of France?',
      streaming: true,
    });

    for await (const chunk of prediction) {
        // {event: "token", data: "hello"}
        console.log(chunk);
    }

  } catch (error) {
    console.error('Error:', error);
  }
}

// ストリーミングテストを実行
test_streaming()
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl https://localhost:3000/api/v1/predictions/{chatflow-id} \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Hello world!",
    "streaming": true
  }'
```
{% endtab %}
{% endtabs %}

```html
event: token
data: Once upon a time...
```

予測のイベントストリームは以下のイベントタイプで構成されています:

| イベント        | 説明                                                                                                                           |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| start           | ストリーミングの開始                                                                                                           |
| token           | 予測が新しいトークン出力をストリーミングしているときに発行                                                                     |
| error           | 予測がエラーを返したときに発行                                                                                                 |
| end             | 予測が終了したときに発行                                                                                                       |
| metadata        | chatId、messageIdなど、関連するフローのすべてのメタデータ。すべてのトークンのストリーミングが終了した後、endイベントの前に発行 |
| sourceDocuments | フローベクトルストアからソースを返すときに発行                                                                                 |
| usedTools       | フローがツールを使用したときに発行                                                                                             |

### Streamlitアプリ

[https://github.com/HenryHengZJ/flowise-streamlit](https://github.com/HenryHengZJ/flowise-streamlit)

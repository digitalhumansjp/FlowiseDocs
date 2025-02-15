---
description: LangChain メモリーノード
---

# メモリー

***

メモリーを使用することで、AIが以前の会話を記憶しているかのようにAIとチャットすることができます。

_<mark style="color:blue;">Human: こんにちは、私はボブです</mark>_

_<mark style="color:orange;">AI: こんにちはボブさん!お会いできて嬉しいです。今日はどのようなお手伝いができますか?</mark>_

_<mark style="color:blue;">Human: 私の名前は何ですか?</mark>_

_<mark style="color:orange;">AI: 先ほど仰っていたように、あなたの名前はボブさんですね。</mark>_

内部的には、これらの会話は配列やデータベースに保存され、LLMにコンテキストとして提供されます。例えば:

```
あなたはOpenAIによって訓練された大規模言語モデルを搭載した人間のアシスタントです。

人間が特定の質問について助けを必要としているか、特定のトピックについて会話をしたいだけかに関わらず、あなたはサポートするためにここにいます。

現在の会話:
{history}
```

### メモリーノード:

* [バッファメモリー](buffer-memory.md)
* [バッファウィンドウメモリー](buffer-window-memory.md)
* [会話サマリーメモリー](conversation-summary-memory.md)
* [会話サマリーバッファメモリー](conversation-summary-buffer-memory.md)
* [DynamoDBチャットメモリー](dynamodb-chat-memory.md)
* [MongoDB Atlasチャットメモリー](mongodb-atlas-chat-memory.md)
* [Redis バックドチャットメモリ](redis-backed-chat-memory.md)
* [Upstash Redisバックドチャットメモリー](upstash-redis-backed-chat-memory.md)
* [Zepメモリー](zep-memory.md)

## 複数ユーザーの会話を分離する

### UIと埋め込みチャット

デフォルトでは、UIと埋め込みチャットは自動的に異なるユーザーの会話を分離します。これは新しい対話ごとに一意の**`chatId`**を生成することで実現されます。この処理はFlowiseによって内部的に処理されます。

### 予測API

一意の**`sessionId`**を指定することで、複数ユーザーの会話を分離できます。

1. すべてのメモリーノードで、入力パラメータ**`Session ID`**を確認できるはずです

<figure><img src="../../../.gitbook/assets/image (76).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Untitled (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

2. `/api/v1/prediction/{your-chatflowid}` POSTボディリクエストで、**`overrideConfig`**内に**`sessionId`**を指定します

```json
{
    "question": "hello!",
    "overrideConfig": {
        "sessionId": "user1"
    }
}
```

### メッセージAPI

* GET `/api/v1/chatmessage/{your-chatflowid}`
* DELETE `/api/v1/chatmessage/{your-chatflowid}`

<table><thead><tr><th>クエリパラメータ</th><th width="192">タイプ</th><th>値</th></tr></thead><tbody><tr><td>sessionId</td><td>string</td><td></td></tr><tr><td>sort</td><td>enum</td><td>ASC または DESC</td></tr><tr><td>startDate</td><td>string</td><td></td></tr><tr><td>endDate</td><td>string</td><td></td></tr></tbody></table>

すべての会話はUIからも可視化および管理できます:

<figure><img src="../../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

OpenAI Assistantの場合、会話の保存には[スレッド](../agents/openai-assistant/threads.md)が使用されます。

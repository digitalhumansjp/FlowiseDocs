# スレッド

[スレッド](https://platform.openai.com/docs/assistants/how-it-works/managing-threads-and-messages)は、OpenAIアシスタントが使用される場合にのみ使用されます。これはアシスタントとユーザー間の会話セッションです。スレッドはメッセージを保存し、コンテンツをモデルのコンテキストに収まるように自動的に切り詰めを処理します。

<figure><img src="../../../../.gitbook/assets/screely-1699896158130.png" alt=""><figcaption></figcaption></figure>

## 複数ユーザーの会話を分離する

### UIと埋め込みチャット

デフォルトでは、UIと埋め込みチャットは複数ユーザーの会話のスレッドを自動的に分離します。これは新しい各インタラクションに対して一意の**`chatId`**を生成することで実現されます。この処理はFlowiseによってバックグラウンドで処理されます。

### 予測API

POST /`api/v1/prediction/{your-chatflowid}`に**`chatId`**を指定します。同じchatIdには同じスレッドが使用されます。

```json
{
    "question": "hello!",
    "chatId": "user1"
}
```

### メッセージAPI

* GET `/api/v1/chatmessage/{your-chatflowid}`
* DELETE `/api/v1/chatmessage/{your-chatflowid}`

**`chatId`**でフィルタリングすることもできます - `/api/v1/chatmessage/{your-chatflowid}?chatId={your-chatid}`

すべての会話はUIからも可視化および管理できます：

<figure><img src="../../../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

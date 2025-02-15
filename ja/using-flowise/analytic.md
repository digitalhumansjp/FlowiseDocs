---
description: チャットフローとエージェントフローを分析してトラブルシューティングする方法を学ぶ
---

# アナリティクス

***

Flowiseは以下の分析プロバイダーと統合されています:

* [LunaryAI](https://lunary.ai/)
* [Langsmith](https://smith.langchain.com/)
* [Langfuse](https://langfuse.com/)
* [LangWatch](https://langwatch.ai/)

## Lunary

[Lunary](https://lunary.ai/)は、LLMチャットボット用の監視・分析プラットフォームです。

Flowiseは、ユーザートレース、フィードバック追跡、会話のリプレイ、詳細なLLM分析をサポートする完全な統合を提供するためにLunaryと提携しています。

Flowiseユーザーは、チェックアウト時にコード`FLOWISEFRIENDS`を使用することで、チームプランで30%の割引を受けることができます。

FlowiseでLunaryをセットアップする方法の詳細については[こちら](https://lunary.ai/docs/integrations/flowise)をご覧ください。

## セットアップ

1. チャットフローまたはエージェントフローの右上隅で、**設定** > **構成**をクリックします

<figure><img src="../.gitbook/assets/analytic-1.webp" alt="構成メニューをクリックするユーザーのスクリーンショット" width="375"><figcaption></figcaption></figure>

2. チャットフロー分析セクションに移動します

<figure><img src="../.gitbook/assets/analytic-2.png" alt="異なる分析プロバイダーを含むチャットフロー分析セクションのスクリーンショット"><figcaption></figcaption></figure>

3. プロバイダーのリストと、その設定フィールドが表示されます

<figure><img src="../.gitbook/assets/image (82).png" alt="認証情報フィールドが展開された分析プロバイダーのスクリーンショット"><figcaption></figcaption></figure>

3. 認証情報やその他の設定詳細を入力し、プロバイダーを**オン**にします

<figure><img src="../.gitbook/assets/image (83).png" alt="有効化された分析プロバイダーのスクリーンショット"><figcaption></figcaption></figure>

## API

UIから分析をオンにすると、[予測API](api.md#prediction-api)のボディで設定を上書きまたは追加設定を提供できます:

```json
{
  "question": "hi there",
  "overrideConfig": {
    "analytics": {
      "langFuse": {
        // langSmith, langFuse, lunary, langWatch
        "userId": "user1"
      }
    }
  }
}
```

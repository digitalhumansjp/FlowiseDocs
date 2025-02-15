# ChatOpenAI

## 前提条件

1. [OpenAI](https://openai.com/)アカウント
2. [APIキー](https://platform.openai.com/api-keys)の作成

## セットアップ

1. **Chat Models** > **ChatOpenAI**ノードをドラッグ

<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

2. **Connect Credential** > **Create New**をクリック

<figure><img src="../../../.gitbook/assets/image_openAI (1).png" alt="" width="278"><figcaption></figcaption></figure>

3. **ChatOpenAI**クレデンシャルを入力

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

4. これで[🎉](https://emojipedia.org/party-popper/)Flowiseで**ChatOpenAIノード**が使用できるようになりました

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## カスタムベースURLとヘッダー

FlowiseはChat OpenAI用のカスタムベースURLとヘッダーの使用をサポートしています。ユーザーはOpenRouter、TogetherAIなどのOpenAI API互換性をサポートする統合を簡単に使用できます。

### TogetherAI

1. TogetherAIの公式[ドキュメント](https://docs.together.ai/docs/openai-api-compatibility#nodejs)を参照
2. TogetherAI APIキーで新しいクレデンシャルを作成
3. ChatOpenAIノードの**Additional Parameters**をクリック
4. Base Pathを変更:

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

### Open Router

1. OpenRouterの公式[ドキュメント](https://openrouter.ai/docs#quick-start)を参照
2. OpenRouter APIキーで新しいクレデンシャルを作成
3. ChatOpenAIノードのAdditional Parametersをクリック
4. Base PathとBase Optionsを変更:

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

## カスタムモデル

ChatOpenAIノードでサポートされていないモデルについては、ChatOpenAI Customを使用できます。これにより、ユーザーは`mistralai/Mixtral-8x7B-Instruct-v0.1`などのモデル名を入力できます。

<figure><img src="../../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

## 画像アップロード

LLMによる画像の分析も可能です。内部的には、Flowiseは[OpenAI Vision](https://platform.openai.com/docs/guides/vision)モデルを使用して画像を処理します。LLMChain、Conversation Chain、ReAct Agent、Conversational Agentでのみ動作します。

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="332"><figcaption></figcaption></figure>

チャットインターフェースに新しい画像アップロードボタンが表示されます:

<figure><img src="../../../.gitbook/assets/Untitled (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

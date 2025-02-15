# ChatGoogleGenerativeAI

## 前提条件

1. [Google](https://accounts.google.com/InteractiveLogin)アカウントの登録
2. [APIキー](https://aistudio.google.com/app/apikey)の作成

## セットアップ

1. **Chat Models** > **ChatGoogleGenerativeAI**ノードをドラッグ

<figure><img src="../../../.gitbook/assets/google_ai/1.png" alt="" width="563"><figcaption></figcaption></figure>

2. **Connect Credential** > **Create New**をクリック

<figure><img src="../../../.gitbook/assets/google_ai/2.png" alt="" width="278"><figcaption></figcaption></figure>

3. **Google AI**クレデンシャルを入力

<figure><img src="../../../.gitbook/assets/google_ai/3.png" alt="" width="563"><figcaption></figcaption></figure>

4. これで[🎉](https://emojipedia.org/party-popper/)Flowiseで**ChatGoogleGenerativeAIノード**が使用できるようになりました

<figure><img src="../../../.gitbook/assets/google_ai/4.png" alt=""><figcaption></figcaption></figure>

## 安全性属性の設定

1. **Additonal Parameters**をクリック

<figure><img src="../../../.gitbook/assets/google_ai/5.png" alt="" width="563"><figcaption></figcaption></figure>

* **Safety Attributes**を設定する際、**Harm Category**と**Harm Block Threshold**の選択数は同じである必要があります。同じでない場合は`Harm Category & Harm Block Threshold are not the same length`というエラーが発生します

* 以下の**Safety Attributes**の組み合わせにより、`Dangerous`は`Low and Above`に、`Harassment`は`Medium and Above`に設定されます

<figure><img src="../../../.gitbook/assets/google_ai/6.png" alt="" width="563"><figcaption></figcaption></figure>

## リソース

* [LangChain JS ChatGoogleGenerativeAI](https://js.langchain.com/docs/integrations/chat/google_generativeai)
* [Google AI for Developers](https://ai.google.dev/)
* [Gemini APIドキュメント](https://ai.google.dev/docs)

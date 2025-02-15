# Azure ChatOpenAI

## 前提条件

1. Azureに[ログイン](https://portal.azure.com/)または[サインアップ](https://azure.microsoft.com/en-us/free/)
2. Azure OpenAIを[作成](https://portal.azure.com/#create/Microsoft.CognitiveServicesOpenAI)し、約10営業日の承認を待つ
3. APIキーは**Azure OpenAI** > **name_azure_openai**をクリック > **Click here to manage keys**をクリックすると利用可能

<figure><img src="../../../.gitbook/assets/azure/azure-general/1.png" alt=""><figcaption></figcaption></figure>

## セットアップ

### Azure ChatOpenAI

1. **Go to Azure OpenaAI Studio**をクリック

<figure><img src="../../../.gitbook/assets/azure/azure-general/2.png" alt=""><figcaption></figcaption></figure>

2. **Deployments**をクリック

<figure><img src="../../../.gitbook/assets/azure/azure-general/3.png" alt=""><figcaption></figcaption></figure>

3. **Create new deployment**をクリック

<figure><img src="../../../.gitbook/assets/azure/azure-general/4.png" alt=""><figcaption></figcaption></figure>

4. 以下のように選択し、**Create**をクリック

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/1.png" alt="" width="558"><figcaption></figcaption></figure>

5. **Azure ChatOpenAI**の作成が完了

* デプロイメント名: `gpt-35-turbo`
* インスタンス名: `右上隅に表示`

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/2.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/azure/azure-general/2.png" alt=""><figcaption></figcaption></figure>

### Flowise

1. **Chat Models** > **Azure ChatOpenAI**ノードをドラッグ

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/3.png" alt="" width="563"><figcaption></figcaption></figure>

2. **Connect Credential** > **Create New**をクリック

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/4.png" alt="" width="421"><figcaption></figcaption></figure>

3. 各詳細(APIキー、インスタンス名、デプロイメント名、[APIバージョン](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference#chat-completions))を**Azure ChatOpenAI**クレデンシャルにコピー＆ペースト

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/5.png" alt="" width="563"><figcaption></figcaption></figure>

4. これで[🎉](https://emojipedia.org/party-popper/)Flowiseで**Azure ChatOpenAIノード**の作成が完了しました

<figure><img src="../../../.gitbook/assets/azure/azure-general/5.png" alt=""><figcaption></figcaption></figure>

## リソース

* [LangChain JS Azure ChatOpenAI](https://js.langchain.com/docs/modules/model_io/models/chat/integrations/azure)
* [Azure OpenAI Service REST APIリファレンス](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference)

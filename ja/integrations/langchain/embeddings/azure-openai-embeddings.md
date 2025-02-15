# Azure OpenAI エンベッディング

## 前提条件

1. Azureに[ログイン](https://portal.azure.com/)または[サインアップ](https://azure.microsoft.com/en-us/free/)
2. Azure OpenAIを[作成](https://portal.azure.com/#create/Microsoft.CognitiveServicesOpenAI)し、約10営業日の承認を待つ
3. APIキーは **Azure OpenAI** > **name_azure_openai**をクリック > **Click here to manage keys**をクリックで確認可能

<figure><img src="../../../.gitbook/assets/azure/azure-general/1.png" alt=""><figcaption></figcaption></figure>

## セットアップ

### Azure OpenAI エンベッディング

1. **Go to Azure OpenaAI Studio**をクリック

<figure><img src="../../../.gitbook/assets/azure/azure-general/2.png" alt=""><figcaption></figcaption></figure>

2. **Deployments**をクリック

<figure><img src="../../../.gitbook/assets/azure/azure-general/3.png" alt=""><figcaption></figcaption></figure>

3. **Create new deployment**をクリック

<figure><img src="../../../.gitbook/assets/azure/azure-general/4.png" alt=""><figcaption></figcaption></figure>

4. 以下のように選択し、**Create**をクリック

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/1.png" alt="" width="559"><figcaption></figcaption></figure>

5. **Azure OpenAI エンベッディング**の作成が完了

* デプロイメント名: `text-embedding-ada-002`
* インスタンス名: `右上コーナー`

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/2.png" alt=""><figcaption></figcaption></figure>

### Flowise

1. **Embeddings** > **Azure OpenAI Embeddings**ノードをドラッグ

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/3.png" alt="" width="563"><figcaption></figcaption></figure>

2. **Connect Credential** > **Create New**をクリック

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/4.png" alt="" width="386"><figcaption></figcaption></figure>

3. 各詳細(APIキー、インスタンス名、デプロイメント名、[APIバージョン](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference#chat-completions))を**Azure OpenAI Embeddings**クレデンシャルにコピー＆ペースト

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/5.png" alt="" width="554"><figcaption></figcaption></figure>

4. これで[🎉](https://emojipedia.org/party-popper/)Flowiseで**Azure OpenAI Embeddings ノード**の作成が完了しました

<figure><img src="../../../.gitbook/assets/azure/azure-general/5.png" alt=""><figcaption></figcaption></figure>

## リソース

* [LangChain JS Azure OpenAI Embeddings](https://js.langchain.com/docs/modules/data_connection/text_embedding/integrations/azure_openai)
* [Azure OpenAI Service REST APIリファレンス](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference)

---
description: FlowiseがLiteLLM Proxyとどのように統合されるかを学ぶ
---

# LiteLLMプロキシ

FlowiseでLiteLLM Proxyを使用することで以下が可能になります:

- Azure OpenAI/LLMエンドポイントのロードバランシング
- OpenAIフォーマットで100以上のLLMを呼び出し
- 仮想キーを使用して予算、レート制限を設定し、使用状況を追跡

## FlowiseでLiteLLM Proxyを使用する方法

### ステップ1: LiteLLM config.yamlファイルでLLMモデルを定義

LiteLLMではすべてのモデルを定義した設定ファイルが必要です - このファイルを`litellm_config.yaml`と呼びます

[litellm configのセットアップ方法の詳細ドキュメントはこちら](https://docs.litellm.ai/docs/proxy/configs)

```yaml
model_list:
  - model_name: gpt-4
    litellm_params:
      model: azure/chatgpt-v-2
      api_base: https://openai-gpt-4-test-v-1.openai.azure.com/
      api_version: "2023-05-15"
      api_key:
  - model_name: gpt-4
    litellm_params:
      model: azure/gpt-4
      api_key:
      api_base: https://openai-gpt-4-test-v-2.openai.azure.com/
  - model_name: gpt-4
    litellm_params:
      model: azure/gpt-4
      api_key:
      api_base: https://openai-gpt-4-test-v-2.openai.azure.com/
```

### ステップ2. litellm proxyを起動

```shell
docker run \
    -v $(pwd)/litellm_config.yaml:/app/config.yaml \
    -p 4000:4000 \
    ghcr.io/berriai/litellm:main-latest \
    --config /app/config.yaml --detailed_debug
```

成功すると、プロキシは`http://localhost:4000/`で実行を開始します

### ステップ3: FlowiseでLiteLLM Proxyを使用

Flowiseでは、**標準のOpenAIノード(Azure OpenAIノードではない)を指定します** -- これはチャットモデル、エンベッディング、LLMなどすべてに適用されます

- `BasePath`をLiteLLM Proxy URL(`http://localhost:4000`：ローカルで実行時)に設定
- 以下のヘッダーを設定 `Authorization: Bearer <your-litellm-master-key>`

---
description: Flowiseで外部API統合を使用する方法を学ぶ
---

# APIとのインタラクション

***

OpenAPI Specification (OAS)は、HTTP APIへの標準的で言語に依存しないインターフェースを定義します。このユースケースの目標は、LLMがどのAPIを呼び出すべきかを自動的に判断しながら、ユーザーとの状態を保持した対話を行うことです。

## OpenAPIチェイン

1. このチュートリアルでは、[Klarna OpenAPI](https://gist.github.com/HenryHengZJ/b60f416c42cb9bcd3160fe797421119a)を使用します。

{% code overflow="wrap" %}
```json
{
  "openapi": "3.0.1",
  "info": {
    "version": "v0",
    "title": "Open AI Klarna product Api"
  },
  "servers": [
    {
      "url": "https://www.klarna.com/us/shopping"
    }
  ],
  "tags": [
    {
      "name": "open-ai-product-endpoint",
      "description": "Open AI Product Endpoint. Query for products."
    }
  ],
  "paths": {
    "/public/openai/v0/products": {
      "get": {
        "tags": [
          "open-ai-product-endpoint"
        ],
        "summary": "API for fetching Klarna product information",
        "operationId": "productsUsingGET",
        "parameters": [
          {
            "name": "countryCode",
            "in": "query",
            "description": "ユーザーの場所に基づく2文字のISO 3166国コード。現在、US、GB、DE、SE、DKのみサポートされています。",
            "required": true,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "q",
            "in": "query",
            "description": "ユーザーが探している製品を見つけるために検索する必要がある非常に小さなカテゴリまたは製品に一致する具体的なクエリ。ユーザーが明示的に求めたものをクエリとして使用します。クエリは可能な限り具体的で、ユーザーが言及した製品名やカテゴリを単数形で含み、最新、最安、予算、プレミアム、高価などの修飾語を含まないようにします。ユーザーが英語以外の言語を話す場合はその要求を英語に翻訳します（例：fia med knuffをludo board gameに翻訳）。",
            "required": true,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "size",
            "in": "query",
            "description": "返される製品の数",
            "required": false,
            "schema": {
              "type": "integer"
            }
          },
          {
            "name": "min_price",
            "in": "query",
            "description": "（オプション）検索対象の製品の最低価格を現地通貨で示します。ユーザーが明示的に述べた場合、またはユーザーの要求と検索する製品の種類の組み合わせから暗黙的に推測された場合。",
            "required": false,
            "schema": {
              "type": "integer"
            }
          },
          {
            "name": "max_price",
            "in": "query",
            "description": "（オプション）検索対象の製品の最高価格を現地通貨で示します。ユーザーが明示的に述べた場合、またはユーザーの要求と検索する製品の種類の組み合わせから暗黙的に推測された場合。",
            "required": false,
            "schema": {
              "type": "integer"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "製品が見つかりました",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProductResponse"
                }
              }
            }
          },
          "503": {
            "description": "1つ以上のサービスが利用できません"
          }
        },
        "deprecated": false
      }
    }
  },
  "components": {
    "schemas": {
      "Product": {
        "type": "object",
        "properties": {
          "attributes": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "name": {
            "type": "string"
          },
          "price": {
            "type": "string"
          },
          "url": {
            "type": "string"
          }
        },
        "title": "Product"
      },
      "ProductResponse": {
        "type": "object",
        "properties": {
          "products": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/Product"
            }
          }
        },
        "title": "ProductResponse"
      }
    }
  }
}
```
{% endcode %}

2. [JSON to YAML コンバーター](https://jsonformatter.org/json-to-yaml)を使用して`.yaml`ファイルとして保存し、**OpenAPI Chain**にアップロードして、質問を行ってテストします。**OpenAPI Chain**はLLMに全仕様を送り、LLMが自動的にAPIの正しいメソッドとパラメータを使用します。

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

3. しかし、通常の会話をしたい場合はそれができません。以下のエラーが表示されます。これは、OpenAPI Chainに次のプロンプトがあるためです。

```
提供されたAPIを使用してこのユーザーのクエリに応答してください
```

APIを必ず見つけてユーザーのクエリに答えるように「強制」したため、OpenAPIと関係のない通常の会話の場合には失敗します。

<figure><img src="../.gitbook/assets/image (134).png" alt="" width="361"><figcaption></figcaption></figure>

この方法は大規模なOpenAPI仕様を持っている場合にはうまく機能しないかもしれません。これは、すべての仕様をLLMに送るメッセージの一部として含めているためです。そのためにLLMが正しいURLやクエリパラメータ、リクエストボディ、その他必要なパラメータを見つけ出す必要があり、仕様が複雑であるほど、LLMが誤って認識する可能性が高くなります。

## ツールエージェント + OpenAPIツールキット

上記のエラーを解決するために、エージェントを使用することができます。OpenAIの公式クックブックより：[OpenAPI仕様による関数呼び出し](https://cookbook.openai.com/examples/function_calling_with_an_openapi_spec)では、すべてのAPIを1つのメッセージとしてLLMに送るのではなく、各APIをそれ自体としてツールに変換することを推奨しています。この方法では、ユーザーのクエリに応じてどのツールを使用するかを決定する能力を持つ、人間のような会話が可能です。

OpenAPIツールキットは、YAMLファイルから各APIを一連のツールに変換します。これにより、ユーザーは各APIごとに[カスタムツール](../integrations/langchain/tools/custom-tool.md)を作成する必要がありません。

1. **ToolAgent**を**OpenAPIツールキット**に接続します。ここでは、OpenAI APIに使うYAML仕様をアップロードします。仕様ファイルはページの下部にあります。

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

2. 試してみましょう！

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

チャットからわかるように、エージェントは通常の会話を実行し、ユーザーのクエリに適したツールを使用することができます。アナリティクスツールを使用している場合、YAMLファイルから変換したツールのリストを見ることができます：

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 結論

必要に応じてAPIとやり取りしながら、ユーザーとの状態を保持した会話を処理できるエージェントを作成しました。このセクションで使用したテンプレートは以下です：

{% file src="../.gitbook/assets/OpenAPI Chatflow.json" %}

{% file src="../.gitbook/assets/OpenAPI Toolkit with ToolAgent Chatflow.json" %}

{% file src="../.gitbook/assets/openai_openapi.yaml" %}

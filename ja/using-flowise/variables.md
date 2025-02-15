---
description: Flowise での変数の使用方法について学ぶ
---

# 変数

***

Flowiseではノードで使用できる変数を作成することができます。変数には静的(Static)と実行時(Runtime)の2種類があります。

### 静的

静的変数は指定された値で保存され、そのまま取得されます。

<figure><img src="../.gitbook/assets/image (13) (1) (1) (1).png" alt="" width="542"><figcaption></figcaption></figure>

### 実行時

変数の値は **.env** ファイルから `process.env` を使用して取得されます。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="537"><figcaption></figcaption></figure>

### APIを通じた変数の上書きまたは設定

変数の値を上書きするには、**Chatflow Configuration** -> **Security** タブから明示的に有効にする必要があります:

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

既存の変数がある場合、APIで提供される変数値が既存の値を上書きします。

```json
{
    "question": "hello",
    "overrideConfig": {
        "vars": {
            "var": "some-override-value"
        }
    }
}
```

### 変数の使用

Flowiseのノードで変数を使用することができます。例えば、**`character`** という名前の変数を作成した場合:

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

この変数は以下のノードの関数内で **`$vars.<変数名>`** として使用できます:

* [Custom Tool](../integrations/langchain/tools/custom-tool.md)
* [Custom Function](../integrations/utilities/custom-js-function.md)
* [Custom Loader](../integrations/langchain/document-loaders/custom-document-loader.md)
* [If Else](../integrations/utilities/if-else.md)

<figure><img src="../.gitbook/assets/image (105).png" alt="" width="283"><figcaption></figcaption></figure>

また、任意のノードのテキスト入力で以下の形式で変数を使用することもできます:

**`{{$vars.<変数名>}}`**

例えば、エージェントのシステムメッセージで:

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2) (1).png" alt="" width="508"><figcaption></figcaption></figure>

プロンプトテンプレートで:

<figure><img src="../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

## リソース

* [関数への変数の渡し方](../integrations/langchain/tools/custom-tool.md#pass-variables-to-function)

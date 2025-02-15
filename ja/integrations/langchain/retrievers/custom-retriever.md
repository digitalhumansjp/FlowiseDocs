---
description: カスタムリトリーバーを使用すると、LLMへのコンテキストのフォーマットをユーザーが指定できます
---

# カスタムリトリーバー

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt="" width="298"><figcaption></figcaption></figure>

デフォルトでは、ベクトルストアからコンテキストが取得される際、以下のような形式になっています：

```json
[
    {
        "pageContent": "これは例です",
        "metadata": {
            "source": "example.pdf"
        }
    },
    {
        "pageContent": "これは例2です",
        "metadata": {
            "source": "example2.txt"
        }
    }
]
```

配列の**pageContent**は文字列として結合され、LLMに送られて処理されます。

しかし、場合によってはソース、リンクなどのメタデータの情報をLLMに提供したい場合があります。そこで**カスタムリトリーバー**の出番です。LLMに返すフォーマットを指定することができます。

例えば、以下のようなフォーマットを使用すると：

```javascript
{{context}}
Source: {{metadata.source}}
```

以下のような結合された文字列が生成されます：

```
これは例です
Source: example.pdf

これは例2です
Source: example2.txt
```

これがLLMに送り返されます。LLMが回答のソースを把握できるようになったので、プロンプトを使用してLLMに引用付きの回答を返すよう指示することができます。

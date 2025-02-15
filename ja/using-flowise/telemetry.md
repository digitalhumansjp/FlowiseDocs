---
description: Flowiseが匿名の使用情報を収集する方法について学ぶ
---

# テレメトリ

***

Flowiseのオープンソースリポジトリには、匿名の使用情報を収集する組み込みのテレメトリがあります。これにより、Flowiseの使用状況をより良く理解し、新機能の開発や問題解決の優先順位付け、Flowiseのパフォーマンスと安定性の向上に向けた取り組みを行うことができます。

{% hint style="warning" %}
<mark style="color:red;">**重要**</mark> - ノードの入出力、メッセージ、認証情報や変数などの機密情報は一切収集しません。イベントのみが送信されます。
{% endhint %}

ソースコードから`telemetry.sendTelemetry`が呼び出されているすべての場所を確認することで、これらの主張を検証できます。

<table><thead><tr><th width="238">イベント</th><th>メタデータ</th></tr></thead><tbody><tr><td>chatflow_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;アプリバージョン>,
    "chatlowId": &#x3C;チャットフローID>,
    "flowGraph": {
        "nodes": [&#x3C;ノードID-1>, &#x3C;ノードID-2>],
        "edges": [
            {
                "source": &#x3C;ノードID-1>,
                "target": &#x3C;ノードID-2>
            }
        ]
    }
}
</code></pre></td></tr><tr><td>tool_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;アプリバージョン>,
    "toolId": &#x3C;ツールID>,
    "toolName": &#x3C;ツール名>
}
</code></pre></td></tr><tr><td>assistant_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;アプリバージョン>,
    "assistantId": &#x3C;アシスタントID>
}
</code></pre></td></tr><tr><td>vector_upserted</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;アプリバージョン>,
    "chatlowId": &#x3C;チャットフローID>,
    "type": "INTERNAL", // EXTERNAL
    "flowGraph": {
        "nodes": [&#x3C;ノードID-1>, &#x3C;ノードID-2>],
        "edges": [
            {
                "source": &#x3C;ノードID-1>,
                "target": &#x3C;ノードID-2>
            }
        ]
    },
    "stopNodeId": &#x3C;ノードID-1>
}
</code></pre></td></tr><tr><td>prediction_sent</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;アプリバージョン>,
    "chatlowId": &#x3C;チャットフローID>,
    "chatId": &#x3C;チャットID>,
    "type": "INTERNAL", // EXTERNAL
    "flowGraph": {
        "nodes": [&#x3C;ノードID-1>, &#x3C;ノードID-2>],
        "edges": [
            {
                "source": &#x3C;ノードID-1>,
                "target": &#x3C;ノードID-2>
            }
        ]
    }
}
</code></pre></td></tr></tbody></table>

## テレメトリの無効化

ユーザーは`.env`ファイルで`DISABLE_FLOWISE_TELEMETRY`を`true`に設定することでテレメトリを無効にできます。

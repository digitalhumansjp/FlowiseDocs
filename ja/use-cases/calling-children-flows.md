---
description: チャットフローツールとカスタムツールの効果的な使用方法を学ぶ
---

# 子フローの呼び出し

***

Flowiseの強力な機能の1つは、フローをツールに変換できることです。例えば、必要なツールをいつ/どのように使用するかを制御するメインフローを持ち、各ツールは特定の目的を実行するように設計されています。

これには以下のような利点があります:

* 各子フローはツールとして独自に実行され、クリーンな出力を可能にする個別のメモリを持ちます
* 各子フローからの詳細な出力を最終的なエージェントに集約することで、より高品質な出力が得られることが多いです

これは以下のツールを使用して実現できます:

* チャットフローツール
* カスタムツール

## チャットフローツール

1. チャットフローを用意します。この例では、複数のチェーンを経由できるChain of Thoughtチャットフローを作成します。

<figure><img src="../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

2. ツールエージェント + チャットフローツールで別のチャットフローを作成します。ツールから呼び出したいチャットフローを選択します。この場合はChain of Thoughtチャットフローです。名前を付け、LLMがこのツールをいつ使用するかを知らせるための適切な説明を加えます:

<figure><img src="../.gitbook/assets/image (35).png" alt="" width="245"><figcaption></figcaption></figure>

3. テストしてみましょう!

<figure><img src="../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>

4. レスポンスから、チャットフローツールの入力と出力を確認できます:

<figure><img src="../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

## カスタムツール

上記と同じ例で、Chain of Thoughtチャットフローの[Prediction API](../using-flowise/api.md#prediction-api)を呼び出すカスタムツールを作成します。

1. 新しいツールを作成します:

<table><thead><tr><th width="180">ツール名</th><th>ツールの説明</th></tr></thead><tbody><tr><td>ideas_flow</td><td>特定の目的を達成する必要がある場合にこのツールを使用します</td></tr></tbody></table>

入力スキーマ:

<table><thead><tr><th>プロパティ</th><th>タイプ</th><th>説明</th><th data-type="checkbox">必須</th></tr></thead><tbody><tr><td>input</td><td>string</td><td>入力質問</td><td>true</td></tr></tbody></table>

<figure><img src="../.gitbook/assets/image (95) (1).png" alt=""><figcaption></figcaption></figure>

ツールのJavaScript関数:

```javascript
const fetch = require('node-fetch');
const url = 'http://localhost:3000/api/v1/prediction/<chatflow-id>'; // 特定のチャットフローIDに置き換える

const body = {
	"question": $input
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

try {
	const response = await fetch(url, options);
	const resp = await response.json();
	return resp.text;
} catch (error) {
	console.error(error);
	return '';
}
```

2. ツールエージェント + カスタムツールを作成します。カスタムツールにステップ1で作成したツールを指定します。

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

3. レスポンスから、カスタムツールの入力と出力を確認できます:

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

## まとめ

この例では、チャットフローツールとカスタムツールを使用して他のチャットフローをツールに変換する2つの方法を実証しました。両者は内部で同じコードロジックを使用しています。

# Nodeの構築

### Gitのインストール

まず、Gitをインストールし、Flowiseリポジトリをクローンします。手順は[Get Started](../getting-started/#for-developers)ガイドで確認できます。

### 構造

FlowiseはすべてのNode統合を`packages/components/nodes`フォルダに分けています。シンプルなツールを作成してみましょう！

### 計算機ツールの作成

`packages/components/nodes/tools`フォルダの下に`Calculator`という名前の新しいフォルダを作成します。次に`Calculator.ts`という名前の新しいファイルを作成します。ファイルの中に基本クラスを書きます。

```javascript
import { INode } from '../../../src/Interface'
import { getBaseClasses } from '../../../src/utils'

class Calculator_Tools implements INode {
    label: string
    name: string
    version: number
    description: string
    type: string
    icon: string
    category: string
    author: string
    baseClasses: string[]

    constructor() {
        this.label = 'Calculator'
        this.name = 'calculator'
        this.version = 1.0
        this.type = 'Calculator'
        this.icon = 'calculator.svg'
        this.category = 'Tools'
        this.author = 'Your Name'
        this.description = 'Perform calculations on response'
        this.baseClasses = [this.type, ...getBaseClasses(Calculator)]
    }
}

module.exports = { nodeClass: Calculator_Tools }
```

すべてのNodeは`INode`基底クラスを実装します。各プロパティの意味の内訳：

<table><thead><tr><th width="271">プロパティ</th><th>説明</th></tr></thead><tbody><tr><td>label</td><td>UIに表示されるNodeの名前</td></tr><tr><td>name</td><td>コードで使用される名前。<strong>キャメルケース</strong>である必要があります</td></tr><tr><td>version</td><td>Nodeのバージョン</td></tr><tr><td>type</td><td>通常はラベルと同じ。UIでこの特定のタイプに接続できるNodeを定義</td></tr><tr><td>icon</td><td>Nodeのアイコン</td></tr><tr><td>category</td><td>Nodeのカテゴリー</td></tr><tr><td>author</td><td>Nodeの作成者</td></tr><tr><td>description</td><td>Nodeの説明</td></tr><tr><td>baseClasses</td><td>Nodeの基底クラス。Nodeは基底コンポーネントから拡張できるため。UIでこのNodeに接続できるNodeを定義</td></tr></tbody></table>

### クラスの定義

コンポーネントクラスが部分的に完成したので、実際のツールクラス、つまり`Calculator`を定義していきましょう。

同じ`Calculator`フォルダに新しいファイルを作成し、`core.ts`という名前を付けます。

```javascript
import { Parser } from "expr-eval"
import { Tool } from "@langchain/core/tools"

export class Calculator extends Tool {
    name = "calculator"
    description = `Useful for getting the result of a math expression. The input to this tool should be a valid mathematical expression that could be executed by a simple calculator.`

    async _call(input: string) {
        try {
            return Parser.evaluate(input).toString()
        } catch (error) {
            return "I don't know how to do that."
        }
    }
}
```

### 仕上げ

`Calculator.ts`ファイルに戻り、`async init`関数を追加して仕上げます。この関数では、先程作成したCalculatorクラスを初期化します。フローが実行されると、各Node内の`init`関数が呼び出され、LLMがこのツールを呼び出すことを決定すると、`_call`関数が実行されます。

```javascript
import { INode } from '../../../src/Interface'
import { getBaseClasses } from '../../../src/utils'
import { Calculator } from './core'

class Calculator_Tools implements INode {
    label: string
    name: string
    version: number
    description: string
    type: string
    icon: string
    category: string
    author: string
    baseClasses: string[]

    constructor() {
        this.label = 'Calculator'
        this.name = 'calculator'
        this.version = 1.0
        this.type = 'Calculator'
        this.icon = 'calculator.svg'
        this.category = 'Tools'
        this.author = 'Your Name'
        this.description = 'Perform calculations on response'
        this.baseClasses = [this.type, ...getBaseClasses(Calculator)]
    }

    async init() {
        return new Calculator()
    }
}

module.exports = { nodeClass: Calculator_Tools }
```

### ビルドと実行

`packages/server`内の`.env`ファイルに新しい環境変数を作成します：

```javascript
SHOW_COMMUNITY_NODES=true
```

これで`pnpm build`と`pnpm start`を使用してコンポーネントを動かすことができます！

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

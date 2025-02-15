# If Else

Flowiseでは、If/Else条件に応じてチャットフローを異なるブランチに分岐させることができます。

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

### 入力変数

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

上の画像から分かるように、`json`出力を持つ任意のノードを受け入れます。例として、カスタム関数、LLMチェーン出力予測、変数の設定/取得などがあります。

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

変数名を付けることができます：

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

この変数は、プレフィックス`$`を付けて[If関数](if-else.md#if-function)と[Else関数](if-else.md#else-function)で使用できます。例：

```
$output
```

### If Else名

ノードの機能を分かりやすくするために名前を付けることができます。

### If関数

これはNodeサンドボックスで実行されるJSコードです。以下が必要です：

* `if`文を含む
* `if`文内で値を返す

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt="" width="312"><figcaption></figcaption></figure>

これにより、正規表現、日付比較など、複雑な比較をより柔軟に行うことができます。

### Else関数

If関数と同様に、値を返す必要があります。この関数は[If関数](if-else.md#if-function)が値を返さない場合にのみ実行されます。

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt="" width="317"><figcaption></figcaption></figure>

### 出力

<figure><img src="../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

[If関数](if-else.md#if-function)が正常に値を返した場合、上図のように**True**出力ドットに渡されます。これにより、ユーザーは次のノードに値を渡すことができます。

それ以外の場合、[Else関数](if-else.md#else-function)からの戻り値が**False**出力ドットに渡されます。

マーケットプレイスのIf Elseテンプレートも参照できます：

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

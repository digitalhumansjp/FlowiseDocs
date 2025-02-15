# 変数の設定/取得

カスタム関数やLLMチェーンを実行する際、同じ処理を再計算/再実行することなく、結果を他のノードで再利用したい場合があります。出力結果を変数として保存し、フローパスの下流にある他のノードで再利用することができます。

<figure><img src="../../.gitbook/assets/savereuse.png" alt=""><figcaption></figcaption></figure>

### 変数の設定

`string、number、boolean、json、array`を出力する任意のノードからの入力を受け取り、変数名を割り当てることができます。

<figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1) (1).png" alt="" width="270"><figcaption></figcaption></figure>

### 変数の取得

後の段階で変数名から変数の値を取得することができます：

<figure><img src="../../.gitbook/assets/image (12) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

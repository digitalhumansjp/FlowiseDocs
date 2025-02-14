# 評価

{% hint style="info" %}
評価機能はCloudプランとEnterpriseプランでのみ利用可能です
{% endhint %}

評価機能は、チャットフロー/エージェントフローアプリケーションのパフォーマンスを監視し理解するのに役立ちます。概要として、評価はチャットフロー/エージェントフローからの一連の入力と対応する出力を取得し、スコアを生成するプロセスです。これらのスコアは、文字列マッチング、数値比較、あるいはLLMを審判として活用するなど、出力を参照結果と比較することで導き出されます。これらの評価はデータセットと評価者を使用して実施されます。

## データセット

データセットは、チャットフロー/エージェントフローの実行に使用される入力と、比較のための対応する出力です。ユーザーは入力と期待される出力を手動で追加するか、「Input」と「Output」の2列を持つCSVファイルをアップロードすることができます。

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

| 入力                     | 出力                         |
| ------------------------ | ---------------------------- |
| イギリスの首都は何ですか | イギリスの首都はロンドンです |
| 1年は何日ありますか      | 1年は365日です               |

## 評価者

評価者はユニットテストのようなものです。評価中、データセットからの入力が選択されたフローで実行され、出力は選択された評価者を使用して評価されます。評価者には3つのタイプがあります：

* **テキストベース**: 文字列ベースのチェック:
  * いずれかを含む
  * すべてを含む
  * いずれも含まない
  * すべてを含まない
  * で始まる
  * で始まらない

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

* **数値ベース:** 数値タイプのチェック:
  * 合計トークン数
  * プロンプトトークン数
  * 完了トークン数
  * APIレイテンシー
  * LLMレイテンシー
  * チャットフローレイテンシー
  * エージェントフローレイテンシー（近日公開）
  * 出力文字数

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

* **LLMベース**: 別のLLMを使用して出力を評価
  * ハルシネーション
  * 正確性

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

## 評価

データセットと評価者の準備ができたら、評価を開始できます。

1.) 評価するデータセットとチャットフローを選択します。複数のデータセットとチャットフローを選択できます。以下の例では、Dataset1のすべての入力が2つのチャットフローに対して実行されます。Dataset1には2つの入力があるため、合計4つの出力が生成され評価されます。

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

2.) 評価者を選択します。この段階で選択できるのは、文字列ベースと数値ベースの評価者のみです。

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

3.) （オプション）LLMベースの評価者を選択します。評価を開始：

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

4.) 評価が完了するまで待ちます：

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

5.) 評価が完了したら、右側のグラフアイコンをクリックして詳細を表示します：

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

上記の3つのチャートは評価の概要を示しています：

* 合格/不合格率
* 使用された平均プロンプトトークン数と完了トークン数
* リクエストの平均レイテンシー

チャートの下の表は、各実行の詳細を示しています。

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (16).png" alt="" width="355"><figcaption></figcaption></figure>

### 評価の再実行

評価に使用されたフローが更新/変更された場合、警告メッセージが表示されます：

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

右上隅の「評価を再実行」ボタンを使用して、同じ評価を再実行できます。異なるバージョンを確認することができます：

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

また、異なるバージョンの結果を表示して比較することもできます：

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

## ビデオチュートリアル

{% embed url="https://youtu.be/kgUttHMkGFg?si=3rLplEp_0TI0p6UV&t=486" %}

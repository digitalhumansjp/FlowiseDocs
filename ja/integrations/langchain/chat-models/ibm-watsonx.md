# IBM Watsonx

## 前提条件

1. [IBM Watsonx](https://www.ibm.com/watsonx)でアカウントを登録
2. 新しいプロジェクトを作成:

<figure><img src="../../../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>

3. プロジェクト作成後、メインダッシュボードに戻り、**Explore foundation models**をクリック:

<figure><img src="../../../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>

4. 使用したいモデルを選択し、Prompt Labで開く:

<figure><img src="../../../.gitbook/assets/image (241).png" alt=""><figcaption></figcaption></figure>

5. 右上隅からView Codeをクリック:

<figure><img src="../../../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

6. `model_id`と`version`パラメータをメモします。この場合、`ibm/granite-3-8b-instruct`とバージョン`2023-05-29`です。
7. 左側のナビゲーションバーをクリックし、Developer accessをクリック

<figure><img src="../../../.gitbook/assets/image (243).png" alt="" width="308"><figcaption></figcaption></figure>

8. `watsonx.ai URL`、`Project ID`をメモし、IBM Cloud Consoleから新しいAPIキーを作成します。
9. この時点で、以下の情報が必要です:
   * Watsonx.ai URL
   * Project ID
   * APIキー
   * モデルのバージョン
   * モデルID

## セットアップ

1. **Chat Models** > **ChatIBMWatsonx**ノードをドラッグ

<figure><img src="../../../.gitbook/assets/image (244).png" alt="" width="306"><figcaption></figcaption></figure>

2. 先ほどのModel IDでModelを入力します。新しいクレデンシャルを作成し、すべての詳細を入力します。

<figure><img src="../../../.gitbook/assets/image (245).png" alt="" width="419"><figcaption></figcaption></figure>

2. これで[🎉](https://emojipedia.org/party-popper/)Flowiseで**ChatIBMWatsonxノード**が使用できるようになりました！

<figure><img src="../../../.gitbook/assets/image (246).png" alt=""><figcaption></figcaption></figure>

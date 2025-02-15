# S3 ファイルローダー

S3ファイルローダーを使用すると、S3からファイルを取得し、[Unstructured](https://unstructured.io/)を使用してベクトル埋め込みに変換できる構造化されたドキュメントオブジェクトに前処理することができます。Unstructuredは、さまざまなファイルタイプに対応するために使用されています。S3上のファイルがPDF、XML、DOCX、CSVのいずれであっても、Unstructuredで処理できます。サポートされているファイルタイプについては[こちら](https://unstructured-io.github.io/unstructured/api.html#supported-file-types)をご覧ください。

## Unstructuredのセットアップ

ホステッドAPIを使用するか、Dockerを使用してローカルで実行するかを選択できます。

* [ホステッドAPI](https://unstructured-io.github.io/unstructured/api.html)
* Docker: `docker run -p 8000:8000 -d --rm --name unstructured-api quay.io/unstructured-io/unstructured-api:latest --port 8000 --host 0.0.0.0`

## S3ファイルローダーのセットアップ

1\. S3ファイルローダーをキャンバスにドラッグ＆ドロップします：

<figure><img src="../../../.gitbook/assets/image (71).png" alt="" width="234"><figcaption></figcaption></figure>

2\. AWS認証情報：AWSアカウントの新しい認証情報を作成します。アクセスキーとシークレットキーが必要です。関連するアカウントにS3バケットポリシーを付与することを忘れないでください。ポリシーガイドは[こちら](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Integrating.Authorizing.IAM.S3CreatePolicy.html)を参照してください。

<figure><img src="../../../.gitbook/assets/image (72).png" alt="" width="551"><figcaption></figcaption></figure>

3. バケット：AWSコンソールにログインしてS3に移動し、バケット名を取得します：

<figure><img src="../../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

4. キー：使用したいオブジェクトをクリックし、キー名を取得します：

<figure><img src="../../../.gitbook/assets/image (75).png" alt="" width="228"><figcaption></figcaption></figure>

5. Unstructured API URL：ホステッドAPIとDockerのどちらを使用しているかに応じて、Unstructured API URLパラメータを変更します。ホステッドAPIを使用している場合は、APIキーも必要です。
6. これでS3のファイルとチャットを開始できます。ドキュメントのチャンク分割はUnstructuredが自動的に処理するため、テキストスプリッターを指定する必要はありません。

<figure><img src="../../../.gitbook/assets/screely-1698767992182.png" alt=""><figcaption></figcaption></figure>

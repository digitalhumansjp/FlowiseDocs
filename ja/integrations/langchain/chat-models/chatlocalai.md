# ChatLocalAI

## LocalAIのセットアップ

[**LocalAI**](https://github.com/go-skynet/LocalAI)は、OpenAI APIの仕様と互換性のあるドロップイン置き換えRESTAPIです。これにより、一般的なハードウェアでLLM（およびそれ以外）をローカルまたはオンプレミスで実行でき、ggml形式と互換性のある複数のモデルファミリーをサポートしています。

FlowiseでChatLocalAIを使用するには、以下の手順に従ってください：

1. ```bash
   git clone https://github.com/go-skynet/LocalAI
   ```
2. ```bash
   cd LocalAI
   ```
3. ```bash
   # モデルをmodels/にコピー
   cp your-model.bin models/
   ```

例：

[gpt4all.io](https://gpt4all.io/index.html)からモデルの1つをダウンロード

```bash
# gpt4all-jをmodels/にダウンロード
wget https://gpt4all.io/models/ggml-gpt4all-j.bin -O models/ggml-gpt4all-j
```

`/models`フォルダ内に、ダウンロードしたモデルが表示されるはずです：

<figure><img src="../../../.gitbook/assets/image (22) (1).png" alt=""><figcaption></figcaption></figure>

サポートされているモデルのリストは[こちら](https://localai.io/model-compatibility/index.html)を参照してください。

4. ```bash
   docker compose up -d --pull always
   ```
5. これでAPIはlocalhost:8080でアクセス可能になります

```bash
# APIのテスト
curl http://localhost:8080/v1/models
# {"object":"list","data":[{"id":"ggml-gpt4all-j.bin","object":"model"}]}
```

## Flowiseのセットアップ

新しいChatLocalAIコンポーネントをキャンバスにドラッグ＆ドロップします：

<figure><img src="../../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

フィールドに入力：

* **Base Path**: LocalAIのベースURL（例：[http://localhost:8080/v1](http://localhost:8080/v1)）
* **Model Name**: 使用したいモデル。LocalAIディレクトリの`/models`フォルダ内にある必要があります。例：`ggml-gpt4all-j.bin`

{% hint style="info" %}
FlowiseとLocalAIの両方をDockerで実行している場合、ベースパスを[http://host.docker.internal:8080/v1](http://host.docker.internal:8080/v1)に変更する必要があるかもしれません。Linuxベースのシステムではhost.docker.internalが利用できないため、デフォルトのdockerゲートウェイを使用する必要があります：[http://172.17.0.1:8080/v1](http://172.17.0.1:8080/v1)
{% endhint %}

以上です！詳細については、LocalAIの[ドキュメント](https://localai.io/basics/getting_started/index.html)を参照してください。

FlowiseでLocalAIを使用する方法を動画でご覧ください

{% embed url="https://youtu.be/0B0oIs8NS9k" %}

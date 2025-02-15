# LocalAI エンベッディング

## LocalAI セットアップ

[**LocalAI**](https://github.com/go-skynet/LocalAI)は、ローカルでの推論のためのOpenAI API仕様と互換性のあるドロップイン置き換えRESTAPIです。これにより、一般的なハードウェアでLLM（およびそれ以外）をローカルまたはオンプレミスで実行でき、ggml形式と互換性のある複数のモデルファミリーをサポートしています。

Flowise内でLocalAI Embeddingsを使用するには、以下の手順に従ってください：

1. ```bash
   git clone https://github.com/go-skynet/LocalAI
   ```
2. <pre class="language-bash"><code class="lang-bash"><strong>cd LocalAI
   </strong></code></pre>
3. LocalAIは、モデルのダウンロード/インストールのための[APIエンドポイント](https://localai.io/api-endpoints/index.html#applying-a-model---modelsapply)を提供しています。この例では、BERTエンベッディングモデルを使用します：

<figure><img src="../../../.gitbook/assets/image (27) (1).png" alt=""><figcaption></figcaption></figure>

4. `/models`フォルダ内にダウンロードされたモデルが表示されるはずです：

<figure><img src="../../../.gitbook/assets/image (23) (1).png" alt=""><figcaption></figcaption></figure>

5. エンベッディングをテストできます：

```bash
curl http://localhost:8080/v1/embeddings -H "Content-Type: application/json" -d '{
    "input": "Test",
    "model": "text-embedding-ada-002"
  }'
```

6. レスポンスは以下のようになります：

<figure><img src="../../../.gitbook/assets/image (29).png" alt="" width="375"><figcaption></figcaption></figure>

## Flowise セットアップ

新しいLocalAIEmbeddingsコンポーネントをキャンバスにドラッグ＆ドロップします：

<figure><img src="../../../.gitbook/assets/image (21) (1) (2).png" alt=""><figcaption></figcaption></figure>

フィールドに入力します：

* **Base Path**: LocalAIのベースURLです（例：[http://localhost:8080/v1](http://localhost:8080/v1)）
* **Model Name**: 使用したいモデル名です。LocalAIディレクトリの`/models`フォルダ内に存在する必要があります。例：`text-embedding-ada-002`

以上です！詳細については、LocalAIの[ドキュメント](https://localai.io/models/index.html#embeddings-bert)を参照してください。

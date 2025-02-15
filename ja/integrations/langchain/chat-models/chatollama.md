# ChatOllama

## 前提条件

1. [Ollama](https://github.com/ollama/ollama)をダウンロードするか、[Docker](https://hub.docker.com/r/ollama/ollama)で実行します。
2. 例えば、以下のコマンドでllama3を使用してDockerインスタンスを起動できます

    ```bash
    docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
    docker exec -it ollama ollama run llama3
    ```

## セットアップ

1. **Chat Models** > **ChatOllama**ノードをドラッグ

<figure><img src="../../../.gitbook/assets/image (139).png" alt="" width="563"><figcaption></figcaption></figure>

2. Ollamaで実行中のモデルを入力します。例：`llama2`。追加パラメータも使用できます：

<figure><img src="../../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

3. これで[🎉](https://emojipedia.org/party-popper/)Flowiseで**ChatOllamaノード**が使用できるようになりました

<figure><img src="../../../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

### 補足

FlowiseとOllamaの両方をDockerで実行している場合、ChatOllamaのベースURLを変更する必要があります。

WindowsとMacOSオペレーティングシステムでは[http://host.docker.internal:8000](http://host.docker.internal:8000/)を指定します。Linuxベースのシステムではhost.docker.internalが利用できないため、デフォルトのdockerゲートウェイを使用する必要があります：[http://172.17.0.1:8000](http://172.17.0.1:8000/)

<figure><img src="../../../.gitbook/assets/image (142).png" alt="" width="292"><figcaption></figcaption></figure>

## リソース

* [LangchainJS ChatOllama](https://js.langchain.com/docs/integrations/chat/ollama)
* [Ollama](https://github.com/ollama/ollama)

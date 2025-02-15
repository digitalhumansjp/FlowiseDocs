# Nvdia NIM

## 前提条件

1. [Nvdia](https://build.nvidia.com/)にログインまたはサインアップ
2. 上部ナビゲーションバーからNIMをクリック:

<figure><img src="../../../.gitbook/assets/image (247).png" alt=""><figcaption></figcaption></figure>

3. 使用したいモデルを検索。ローカルにダウンロードするためにDockerを使用します:

<figure><img src="../../../.gitbook/assets/image (248).png" alt=""><figcaption></figcaption></figure>

4. Dockerセットアップの手順に従います。Dockerイメージをプルするには、まずAPIキーを取得する必要があります:

<figure><img src="../../../.gitbook/assets/image (249).png" alt="" width="563"><figcaption></figcaption></figure>

## Flowise

1. **Chat Models** > **Chat NvdiaNIM**ノードをドラッグ

<figure><img src="../../../.gitbook/assets/image (250).png" alt=""><figcaption></figcaption></figure>

2. Nvdiaホステッドエンドポイントを使用している場合は、APIキーが必要です。**Connect Credential** > **Create New**をクリック。ただし、ローカルセットアップを使用している場合、これはオプションです。

<div align="left"><figure><img src="../../../.gitbook/assets/image (251).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../../.gitbook/assets/Screenshot 2024-12-23 180712.png" alt=""><figcaption></figcaption></figure></div>

3. モデル名を入力すれば[🎉](https://emojipedia.org/party-popper/)、**Nvdia NIMノード**がFlowiseで使用できるようになりました！

<figure><img src="../../../.gitbook/assets/image (252).png" alt=""><figcaption></figcaption></figure>

## リソース

* [Nvida LLM入門](https://docs.nvidia.com/nim/large-language-models/latest/getting-started.html)
* [Nvdia NIM](https://build.nvidia.com/microsoft/phi-3-mini-4k?snippet_tab=Docker)

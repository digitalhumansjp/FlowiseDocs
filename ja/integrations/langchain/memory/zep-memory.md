# Zepメモリー

[Zep](https://github.com/getzep/zep)はLLMアプリケーション用の長期メモリーストアです。LLMアプリ/チャットボットの履歴を保存、要約、埋め込み、インデックス化、エンリッチ化し、シンプルで低レイテンシーなAPIを通じて公開します。

## RenderへのZepデプロイガイド

[Render](https://render.com/)や[Flyio](https://fly.io/)などのクラウドサービスに簡単にZepをデプロイできます。ローカルでテストしたい場合は、[クイックガイド](https://github.com/getzep/zep#quick-start)に従ってDockerコンテナを起動することもできます。

この例では、Renderにデプロイします。

1. [Zepリポジトリ](https://github.com/getzep/zep#quick-start)に移動し、**Deploy to Render**をクリックします
2. RenderのBlueprintページに移動するので、**Create New Resources**をクリックします

<figure><img src="../../../.gitbook/assets/image (21) (1).png" alt=""><figcaption></figcaption></figure>

3. デプロイが完了すると、ダッシュボードに3つのアプリケーションが作成されます

<figure><img src="../../../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>

4. **zep**という名前の最初のものをクリックし、デプロイされたURLをコピーします

<figure><img src="../../../.gitbook/assets/image (38) (1).png" alt=""><figcaption></figcaption></figure>

## Digital Ocean(Docker経由)へのZepデプロイガイド

1. リポジトリをクローン

```bash
git clone https://github.com/getzep/zep.git
cd zep
nano .env
```

2. .ENVファイルにOpenAI APIキーを追加

```bash
ZEP_OPENAI_API_KEY=
```

```bash
docker compose up -d --build
```

3. ポート8000へのファイアウォールアクセスを許可

```bash
sudo ufw allow from any to any port 8000 proto tcp
ufw status numbered
```

Digital Oceanのダッシュボードで別のファイアウォールを使用している場合は、ポート8000がそこにも追加されていることを確認してください

## Flowise UIでの使用

1. Flowiseアプリケーションに戻り、新しいキャンバスを作成するか、マーケットプレイスからテンプレートを使用します。この例では、**Simple Conversational Chain**を使用します

<figure><img src="../../../.gitbook/assets/Untitled (3) (1).png" alt=""><figcaption></figcaption></figure>

2. **Buffer Memory**を**Zep Memory**に置き換えます。次に、**Base URL**を上でコピーしたZep URLに置き換えます

<figure><img src="../../../.gitbook/assets/Untitled (5).png" alt=""><figcaption></figcaption></figure>

3. チャットフローを保存し、会話が記憶されているかテストします

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

4. チャット履歴をクリアしてみると、以前の会話を覚えていないことがわかります

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Zep認証

Zepでは、JWTを使用してインスタンスを保護できます。ここでは[zepcli](https://github.com/getzep/zepcli/releases)コマンドラインユーティリティを使用します。

#### 1. シークレットとJWTトークンの生成

ZepCLIをダウンロードした後:

LinuxまたはMacOSの場合

```
./zepcli -i
```

Windowsの場合

```
zepcli.exe -i
```

まず、SECRETトークンが表示されます:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

次に、JWTトークンが表示されます:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### 2. 認証環境変数の設定

Zepサーバー環境で以下の環境変数を設定します:

```
ZEP_AUTH_REQUIRED=true
ZEP_AUTH_SECRET=<上で生成したシークレット>
```

#### 3. Flowiseでの認証情報の設定

Zep用の新しい認証情報を追加し、APIキーフィールドにJWTトークンを入力します:

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 4. Zepノードで作成した認証情報を使用

Zepノードの接続認証情報で、作成した認証情報を選択します。これで完了です！

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

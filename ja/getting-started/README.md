# 始めましょう

***

## クラウド

セルフホスティングは、インスタンスのセットアップ、データベースのバックアップ、更新の管理に多くの技術的スキルを必要とします。サーバー管理に不慣れで、単にウェブアプリを使いたい場合は、[Flowise Cloud](https://flowiseai.com/join) の利用をお勧めします。

## クイックスタート

{% hint style="info" %}
事前条件: マシンに [NodeJS](https://nodejs.org/en/download) がインストールされていることを確認。Node `v18.15.0`または`v20`以上がサポートされています。
{% endhint %}

NPMを使用してFlowiseをローカルにインストールします。

1. Flowiseのインストール:

```bash
npm install -g flowise
```

特定のバージョンをインストールすることもできます。利用可能な[バージョン](https://www.npmjs.com/package/flowise?activeTab=versions)を参照してください。

```
npm install -g flowise@x.x.x
```

2. Flowiseの開始:

```bash
npx flowise start
```

3. 開く: [http://localhost:3000](http://localhost:3000)

***

## Docker

Dockerを使用してFlowiseをデプロイするには2つの方法があります:

### Docker Compose

1. プロジェクトのルートにある `docker フォルダ` に移動
2. `.env.example` ファイルをコピーし、`.env` という名前の別のファイルとして貼り付けます
3. 実行:

```bash
docker compose up -d
```

4. 開く: [http://localhost:3000](http://localhost:3000)
5. コンテナを停止するには、以下を実行:

```bash
docker compose stop
```

### Docker イメージ

1. イメージをビルド:

```bash
docker build --no-cache -t flowise .
```

2. イメージを実行:

```bash
docker run -d --name flowise -p 3000:3000 flowise
```

3. イメージを停止:

```bash
docker stop flowise
```

***

## 開発者向け

Flowiseは単一のモノレポジトリに3つの異なるモジュールがあります:

* **サーバー**: APIロジックを提供するNodeのバックエンド
* **UI**: Reactフロントエンド
* **コンポーネント**: 統合コンポーネント

### 必要条件

[PNPM](https://pnpm.io/installation) をインストールします。

```bash
npm i -g pnpm
```

### セットアップ1

PNPMを使用した簡単なセットアップ:

1. リポジトリをクローン

```bash
git clone https://github.com/FlowiseAI/Flowise.git
```

2. リポジトリフォルダに移動

```bash
cd Flowise
```

3. すべてのモジュールの依存関係をインストール:

```bash
pnpm install
```

4. コードをビルド:

```bash
pnpm build
```

アプリを開始: [http://localhost:3000](http://localhost:3000)

```bash
pnpm start
```

### セットアップ2

プロジェクト貢献者向けのステップバイステップセットアップ:

1. 公式 [Flowise Github リポジトリ](https://github.com/FlowiseAI/Flowise) をフォーク
2. フォークしたリポジトリをクローン
3. 新しいブランチを作成、[ガイド](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository) を参照。命名規則:
   * 機能ブランチの場合: `feature/<新しい機能>`
   * バグ修正ブランチの場合: `bugfix/<新しいバグ修正>`。
4. 作成したブランチに切り替える
5. リポジトリフォルダに移動:

```bash
cd Flowise
```

6. すべてのモジュールの依存関係をインストール:

```bash
pnpm install
```

7. コードをビルド:

```bash
pnpm build
```

8. アプリを開始: [http://localhost:3000](http://localhost:3000)

```bash
pnpm start
```

9. 開発用ビルドの場合:

* `.env` ファイルを作成し、`packages/ui` に `PORT` を指定 (`.env.example` を参照)
* `.env` ファイルを作成し、`packages/server` に `PORT` を指定 (`.env.example` を参照)

```bash
pnpm dev
```

* `packages/ui` または `packages/server` で行った変更は [http://localhost:8080](http://localhost:8080/) に反映されます
* `packages/components` の変更については、変更を反映するために再度ビルドが必要です
* すべての変更を行った後に、以下を実行します:

    ```bash
    pnpm build
    ```

    そして

    ```bash
    pnpm start
    ```

    本番環境で問題なく動作することを確認します。

***

## エンタープライズ向け

エンタープライズプランには、別のリポジトリとDockerイメージがあります。

両方へのアクセス権が付与されたら、セットアップは[#setup-1](./#setup-1 "mention")と同じです。アプリを開始する前に、エンタープライズパラメータの値を `.env` ファイルに入力する必要があります。必要な変更については `.env.example` を参照してください。

次の環境変数の値については support@flowiseai.com に連絡してください:

```
LICENSE_URL
FLOWISE_EE_LICENSE_KEY
```

Dockerのインストールについて:

```bash
cd docker
cd enterprise
docker compose up -d
```

***

## 詳しく学習する

このビデオチュートリアルでは、LeonがFlowiseの概要を説明し、ローカルマシンでのセットアップ方法を説明しています。

{% embed url="https://youtu.be/nqAK_L66sIQ" %}

## コミュニティガイド

* [LLMアプリケーション構築のためのFlowise / LangChainによる導入\[実践\]](https://volcano-ice-cd6.notion.site/Introduction-to-Practical-Building-LLM-Applications-with-Flowise-LangChain-03d6d75bfd20495d96dfdae964bea5a5)
* [Flowise / LangChainによるLLMアプリケーション構築\[実践\]入門](https://volcano-ice-cd6.notion.site/Flowise-LangChain-LLM-e106bb0f7e2241379aad8fa428ee064a)

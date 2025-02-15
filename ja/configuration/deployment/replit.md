---
description: ReplitへのFlowiseのデプロイ方法を学ぶ
---

# Replit

***

1. [Replit](https://replit.com/~)にサインイン
2. 新しい**Repl**を作成。テンプレートとして**Node.js**を選択し、任意の**Title**を入力。

<figure><img src="../../.gitbook/assets/image (18) (1) (2) (1).png" alt="" width="551"><figcaption></figcaption></figure>

3. 新しいReplが作成されたら、左側のサイドバーでSecretをクリック：

<figure><img src="../../.gitbook/assets/image (2) (4) (1).png" alt="" width="219"><figcaption></figcaption></figure>

4. PuppeteerとPlaywrightライブラリのChromiumダウンロードをスキップするために3つのSecretsを作成。

<table><thead><tr><th width="403">Secrets</th><th>Value</th></tr></thead><tbody><tr><td>PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD</td><td>1</td></tr><tr><td>PUPPETEER_SKIP_DOWNLOAD</td><td>true</td></tr><tr><td>PUPPETEER_SKIP_CHROMIUM_DOWNLOAD</td><td>true</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (5) (3).png" alt="" width="535"><figcaption></figcaption></figure>

5. Shellタブに切り替えることができます

<figure><img src="../../.gitbook/assets/image (13) (2) (1).png" alt="" width="539"><figcaption></figcaption></figure>

6. Shellターミナルウィンドウに`npm install -g flowise`と入力。Node.jsのバージョンの互換性エラーが発生する場合は、`yarn global add flowise --ignore-engines`コマンドを使用してください

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="530"><figcaption></figcaption></figure>

7. 続いて`npx flowise start`を実行

<figure><img src="../../.gitbook/assets/image (17) (1) (2).png" alt="" width="533"><figcaption></figcaption></figure>

8. これでReplitでFlowiseが表示されるはずです！

<figure><img src="../../.gitbook/assets/image (15) (3).png" alt="" width="545"><figcaption></figcaption></figure>

9. [アプリレベルの認証](broken-reference/)を有効にしたい場合は、コマンドを以下のように変更：

```bash
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

10. ログインページが表示されます。設定したユーザー名とパスワードでログインしてください。

<figure><img src="../../.gitbook/assets/image (12) (2) (1).png" alt=""><figcaption></figcaption></figure>

---
description: チャットウィジェットのカスタマイズと埋め込み方法について学ぶ
---

# 埋め込み（エンベッド）

***

チャットウィジェットは簡単にウェブサイトに追加できます。提供されたウィジェットスクリプトをコピーして、HTMLファイルの`<body>`と`</body>`タグの間の任意の場所に貼り付けるだけです。

<figure><img src="../.gitbook/assets/image (8) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

## ウィジェットのセットアップ

以下の動画は、ウィジェットスクリプトを任意のウェブページに挿入する方法を示しています。

{% embed url="https://github.com/FlowiseAI/Flowise/assets/26460777/c128829a-2d08-4d60-b821-1e41a9e677d0" %}

## 特定バージョンの使用

flowise-embedの`web.js`の使用バージョンを指定することができます。バージョンの完全なリストは次のURLで確認できます: [https://www.npmjs.com/package/flowise-embed](https://www.npmjs.com/package/flowise-embed)

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed@<some-version>/dist/web.js';
  Chatbot.init({
    chatflowid: 'your-chatflowid-here',
    apiHost: 'your-apihost-here',
  })
</script>
```

{% hint style="warning" %}
Flowise **v2.1.0**では、ストリーミングの動作方法を変更しました。Flowiseのバージョンがそれより低い場合、埋め込まれたチャットボットがメッセージを受信できない可能性があります。

Flowiseを**v2.1.0**以上にアップデートするか、

何らかの理由でFlowiseをアップデートしたくない場合は、[Flowise-Embed](https://www.npmjs.com/package/flowise-embed?activeTab=versions)の最新の**v1.x.x**バージョンを指定することができます。最後にメンテナンスされた`web.js`のバージョンは**v1.3.14**です。

例えば:

`https://cdn.jsdelivr.net/npm/flowise-embed@1.3.14/dist/web.js`
{% endhint %}

## チャットフローの設定

`chatflowConfig` JSONオブジェクトを渡して、既存の設定を上書きすることができます。これはAPIの[#override-config](api.md#override-config "mention")と同じです。

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
  Chatbot.init({
    chatflowid: 'your-chatflowid-here',
    apiHost: 'your-apihost-here',
    chatflowConfig: {
      "sessionId": "123",
      "returnSourceDocuments": true
    }
  })
</script>
```

## オブザーバー設定

チャットボット内のシグナル観察に基づいて、親要素でコードを実行することができます。

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
  Chatbot.init({
    chatflowid: 'your-chatflowid-here',
    apiHost: 'your-apihost-here',
    observersConfig: {
      // ユーザー入力が変更された場合
      observeUserInput: (userInput) => {
        console.log({ userInput });
      },
      // ボットのメッセージスタックが変更された場合
      observeMessages: (messages) => {
        console.log({ messages });
      },
      // ボットのローディング状態が変更された場合
      observeLoading: (loading) => {
        console.log({ loading });
      },
    },
  })
</script>
```

## テーマ

テーマプロパティを使用して、埋め込みチャットボットの外観を完全に変更し、ツールチップ、免責事項、カスタムウェルカムメッセージなどの機能を有効にすることができます。これにより、ウィジェットの見た目と操作感を以下の項目を含めて詳細にカスタマイズできます：

* **ボタン:** 位置、サイズ、色、アイコン、ドラッグ＆ドロップの動作、自動開閉。
* **ツールチップ:** 表示/非表示、メッセージテキスト、背景色、テキスト色、フォントサイズ。
* **免責事項:** タイトル、メッセージ、テキスト色、ボタン色、背景色（ぼかしオーバーレイオプションを含む）。
* **チャットウィンドウ:** タイトル、エージェント/ユーザーメッセージの表示、ウェルカム/エラーメッセージ、背景色/画像、寸法、フォントサイズ、スターター プロンプト、HTMLレンダリング、メッセージのスタイル（色、アバター）、テキスト入力の動作（プレースホルダー、色、文字数制限、サウンド）、フィードバックオプション、日付/時刻表示、フッターのカスタマイズ。
* **カスタムCSS:** より細かい外観の制御のためにCSSコードを直接注入し、必要に応じてデフォルトのスタイルを上書きすることができます（[以下の手順ガイドを参照](embed.md#custom-css-modification)）。

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
  Chatbot.init({
    chatflowid: 'your-chatflowid-here',
    apiHost: 'your-apihost-here',
    theme: {
      button: {
        backgroundColor: '#3B81F6',
        right: 20,
        bottom: 20,
        size: 48, // small | medium | large | number
        dragAndDrop: true,
        iconColor: 'white',
        customIconSrc: 'https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/svg/google-messages.svg',
        autoWindowOpen: {
          autoOpen: true, //自動ウィンドウ開閉を制御するパラメーター
          openDelay: 2, //遅延時間（秒）のオプションパラメーター
          autoOpenOnMobile: false, //モバイルでの自動ウィンドウ開閉を制御するパラメーター
        },
      },
      tooltip: {
        showTooltip: true,
        tooltipMessage: 'こんにちは 👋!',
        tooltipBackgroundColor: 'black',
        tooltipTextColor: 'white',
        tooltipFontSize: 16,
      },
      disclaimer: {
        title: '免責事項',
        message: 'このチャットボットを使用することで、<a target="_blank" href="https://flowiseai.com/terms">利用規約</a>に同意したものとみなされます',
        textColor: 'black',
        buttonColor: '#3b82f6',
        buttonText: 'チャットを開始',
        buttonTextColor: 'white',
        blurredBackgroundColor: 'rgba(0, 0, 0, 0.4)', //チャットインターフェースに重ねるぼかし背景の色
        backgroundColor: 'white',
      },
      customCSS: ``, // カスタムCSSスタイルを追加。デフォルトスタイルを上書きするには!importantを使用
      chatWindow: {
        showTitle: true,
        showAgentMessages: true,
        title: 'Flowise Bot',
        titleAvatarSrc: 'https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/svg/google-messages.svg',
        welcomeMessage: 'こんにちは！これはカスタムウェルカムメッセージです',
        errorMessage: 'これはカスタムエラーメッセージです',
        backgroundColor: '#ffffff',
        backgroundImage: '画像のパスまたはリンクを入力', // 設定すると、チャットウィンドウの背景色が上書きされます
        height: 700,
        width: 400,
        fontSize: 16,
        starterPrompts: ['ボットとは何ですか？', 'あなたは誰ですか？'], // チャットフローで設定されたスターター プロンプトを上書きします
        starterPromptFontSize: 15,
        clearChatOnReload: false, // trueに設定すると、ページのリロード時にチャットがクリアされます
        sourceDocsTitle: 'ソース:',
        renderHTML: true,
        botMessage: {
          backgroundColor: '#f7f8ff',
          textColor: '#303235',
          showAvatar: true,
          avatarSrc: 'https://raw.githubusercontent.com/zahidkhawaja/langchain-chat-nextjs/main/public/parroticon.png',
        },
        userMessage: {
          backgroundColor: '#3B81F6',
          textColor: '#ffffff',
          showAvatar: true,
          avatarSrc: 'https://raw.githubusercontent.com/zahidkhawaja/langchain-chat-nextjs/main/public/usericon.png',
        },
        textInput: {
          placeholder: '質問を入力してください',
          backgroundColor: '#ffffff',
          textColor: '#303235',
          sendButtonColor: '#3B81F6',
          maxChars: 50,
          maxCharsWarningMessage: '文字数制限を超えています。50文字以下で入力してください。',
          autoFocus: true, // 使用しない場合、モバイルでは無効、デスクトップでは有効になります。trueは両方で有効、falseは両方で無効になります。
          sendMessageSound: true,
          // sendSoundLocation: "send_message.mp3", // 使用しない場合、sendSoundMessageがtrueの場合はデフォルトの効果音が再生されます。
          receiveMessageSound: true,
          // receiveSoundLocation: "receive_message.mp3", // 使用しない場合、receiveSoundMessageがtrueの場合はデフォルトの効果音が再生されます。
        },
        feedback: {
          color: '#303235',
        },
        dateTimeToggle: {
          date: true,
          time: true,
        },
        footer: {
          textColor: '#303235',
          text: 'Powered by',
          company: 'Flowise',
          companyLink: 'https://flowiseai.com',
        },
      },
    },
  });
</script>
```

**注意:** 完全な[設定リスト](https://github.com/FlowiseAI/FlowiseChatEmbed#configuration)を参照してください。

## カスタムコード変更

埋め込みチャットウィジェットのソースコード全体を変更するには、以下の手順に従ってください：

1. [Flowise Chat Embed](https://github.com/FlowiseAI/FlowiseChatEmbed)リポジトリをフォークします
2. `yarn install`を実行して必要な依存関係をインストールします
3. コードを任意に変更します
4. `yarn build`を実行して変更を反映させます
5. 変更をフォークしたリポジトリにプッシュします
6. 以下のように、カスタマイズした`web.js`を埋め込みチャットとして使用できます：

`username`をあなたのGithubユーザー名に、`forked-repo`をフォークしたリポジトリ名に置き換えてください。

```html
<script type="module">
      import Chatbot from "https://cdn.jsdelivr.net/gh/username/forked-repo/dist/web.js"
      Chatbot.init({
          chatflowid: "your-chatflowid-here",
          apiHost: "your-apihost-here",
      })
</script>
```

<figure><img src="../.gitbook/assets/image (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

```html
<script type="module">
      import Chatbot from "https://cdn.jsdelivr.net/gh/HenryHengZJ/FlowiseChatEmbed-Test/dist/web.js"
      Chatbot.init({
          chatflowid: "your-chatflowid-here",
          apiHost: "your-apihost-here",
      })
</script>
```

{% hint style="info" %}
jsdelivrの代替としてunpkgがあります。以下は例です：

```
https://unpkg.com/flowise-embed/dist/web.js
```
{% endhint %}

## カスタムCSS変更

カスタムの`web.js`ファイルを必要とせずに、埋め込みチャットウィジェットにカスタムCSSを直接追加できるようになりました（v2.0.8以降が必要）。これにより以下が可能になります：

* 埋め込まれた各チャットボットに独自の見た目と操作感を与える
* 公式の`web.js`を使用—スタイリングのためのカスタムビルドやホスティングが不要
* スタイルをすぐに更新できる

使用方法は以下の通りです：

```html
<script src="https://cdn.jsdelivr.net/gh/FlowiseAI/FlowiseChatEmbed@main/dist/web.js"></script>
<script>
  Chatbot.init({
    chatflowid: "your-chatflowid-here",
    apiHost: "your-apihost-here",
    theme: {
      // ... その他のテーマ設定
      customCSS: `
        /* カスタムCSSをここに記述 */
        /* デフォルトスタイルを上書きするには!importantを使用 */
      `,
    }
  });
</script>
```

## CORS

埋め込みチャットウィジェットを使用する際に、以下のようなCORS関連の問題に遭遇する可能性があります：

{% hint style="danger" %}
Access to fetch at 'https://<your-flowise.com>/api/v1/prediction/' from origin 'https://<your-flowise.com>' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
{% endhint %}

これを解決するには、以下の環境変数を指定してください：

```
CORS_ORIGINS=*
IFRAME_ORIGINS=*
```

例えば、`npx flowise start`を使用している場合：

```
npx flowise start --CORS_ORIGINS=* --IFRAME_ORIGINS=*
```

Dockerを使用している場合は、環境変数を`Flowise/docker/.env`内に配置します。

ローカルのGitクローンを使用している場合は、環境変数を`Flowise/packages/server/.env`内に配置します。

## ビデオチュートリアル

以下の2つのビデオでは、Flowiseウィジェットをウェブサイトに埋め込む方法を学ぶことができます。

{% embed url="https://youtu.be/4paQ2wObDQ4" %}

{% embed url="https://youtu.be/XOeCV1xyN48" %}

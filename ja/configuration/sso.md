# SSO

{% hint style="info" %}
SSOはエンタープライズプランでのみ利用可能です
{% endhint %}

Flowiseは[OIDC](https://openid.net/)をサポートしており、ユーザーがアプリケーションにアクセスする際にシングルサインオン(SSO)を使用できます。現在、[組織管理者](../using-flowise/workspaces.md#setting-up-admin-account)のみがSSO設定を構成できます。

## Microsoft

1. Azureポータルで、Microsoft Entra IDを検索します：

<figure><img src="../.gitbook/assets/image (193).png" alt=""><figcaption></figcaption></figure>

2. 左側のバーからアプリの登録をクリックし、新規登録をクリックします：

<figure><img src="../.gitbook/assets/image (194).png" alt=""><figcaption></figcaption></figure>

3. アプリ名を入力し、シングルテナントを選択します：

<figure><img src="../.gitbook/assets/image (195).png" alt=""><figcaption></figcaption></figure>

4. アプリが作成されたら、アプリケーション(クライアント)IDとディレクトリ(テナント)IDをメモします：

<figure><img src="../.gitbook/assets/image (196).png" alt=""><figcaption></figcaption></figure>

5. 左側のバーで、証明書とシークレット -> 新しいクライアントシークレット -> 追加をクリックします：

<figure><img src="../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

6. シークレットが作成されたら、シークレットIDでは<mark style="color:red;">なく</mark>、値をコピーします：

<figure><img src="../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

7. 左側のバーで、認証 -> プラットフォームを追加 -> Webをクリックします：

<figure><img src="../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>

8. リダイレクトURIを入力します。これはホスティング方法によって変更する必要があります：`http[s]://[your-flowise-instance.com]/api/v1/azure/callback`：

<figure><img src="../.gitbook/assets/image (218).png" alt="" width="514"><figcaption></figcaption></figure>

9. 新しいリダイレクトURIが作成されたことを確認できます：

<figure><img src="../.gitbook/assets/image (219).png" alt=""><figcaption></figcaption></figure>

10. Flowiseアプリに戻り、組織管理者としてログインします。左側のバーからSSO設定に移動します。ステップ4のAzureテナントIDとクライアントID、ステップ6のクライアントシークレットを入力します。設定のテストをクリックして、接続が正常に確立できるか確認します：

<figure><img src="../.gitbook/assets/image (220).png" alt="" width="563"><figcaption></figcaption></figure>

11. 最後に、有効化して保存します：

<figure><img src="../.gitbook/assets/image (221).png" alt="" width="563"><figcaption></figcaption></figure>

12. ユーザーがSSOを使用してサインインする前に、まず招待する必要があります。手順については[SSOサインインのためのユーザー招待](sso.md#inviting-users-for-sso-sign-in)を参照してください。招待されたユーザーは、Azureのディレクトリユーザーの一部である必要もあります。

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

## Google

ウェブサイトでGoogleサインインを有効にするには、まずGoogleのAPIクライアントIDを設定する必要があります。以下の手順で設定を行います：

1. [Google APIs console](https://console.developers.google.com/apis)の**認証情報**ページを開きます。
2. **認証情報を作成** > **OAuthクライアントID**をクリックします

<figure><img src="../.gitbook/assets/image (224).png" alt="" width="563"><figcaption></figcaption></figure>

3\. **ウェブアプリケーション**を選択します：

<figure><img src="../.gitbook/assets/image (225).png" alt="" width="504"><figcaption></figcaption></figure>

4\. リダイレクトURIを入力します。これはホスティング方法によって変更する必要があります：`http[s]://[your-flowise-instance.com]/api/v1/google/callback`：

<figure><img src="../.gitbook/assets/image (226).png" alt="" width="563"><figcaption></figcaption></figure>

5\. 作成後、クライアントIDとシークレットを取得します：

<figure><img src="../.gitbook/assets/image (227).png" alt=""><figcaption></figcaption></figure>

6\. Flowiseアプリに戻り、クライアントIDとシークレットを追加します。接続をテストして保存します。

<figure><img src="../.gitbook/assets/image (228).png" alt="" width="563"><figcaption></figcaption></figure>

## Auth0

1. [Auth0](https://auth0.com/)でアカウントを登録し、新しいアプリケーションを作成します

<figure><img src="../.gitbook/assets/image (229).png" alt=""><figcaption></figcaption></figure>

2. **Regular Web Application**を選択します：

<figure><img src="../.gitbook/assets/image (230).png" alt=""><figcaption></figcaption></figure>

3. 名前、説明などのフィールドを設定します。**ドメイン**、**クライアントID**、**クライアントシークレット**をメモしておきます。

<figure><img src="../.gitbook/assets/image (231).png" alt=""><figcaption></figcaption></figure>

4\. アプリケーションURIを入力します。これはホスティング方法によって変更する必要があります：`http[s]://[your-flowise-instance.com]/api/v1/auth0/callback`：

<figure><img src="../.gitbook/assets/image (232).png" alt=""><figcaption></figcaption></figure>

5. APIタブで、Auth0 Management APIが以下の権限で有効になっていることを確認します
   * read:users
   * read:client\_grants

<figure><img src="../.gitbook/assets/image (233).png" alt=""><figcaption></figcaption></figure>

6\. Flowiseアプリに戻り、ドメイン、クライアントIDとシークレットを入力します。設定をテストして保存します。

<figure><img src="../.gitbook/assets/image (234).png" alt="" width="563"><figcaption></figcaption></figure>

## SSOサインインのためのユーザー招待

新しいユーザーがSSOを使用してログインできるようにするには、Flowiseアプリケーションに新しいユーザーを招待する必要があります。これは招待されたユーザーのロール/ワークスペースの記録を保持するために不可欠です。環境変数の設定については[ユーザーの招待](../using-flowise/workspaces.md#invite-user)セクションを参照してください。

組織管理者は招待ユーザーのログインタイプを選択できます：

<figure><img src="../.gitbook/assets/image (213).png" alt=""><figcaption></figcaption></figure>

* SSO: 招待ユーザーはSSOでのみログイン可能
* メール/パスワード: 招待ユーザーはメール/パスワードでのみログイン可能

招待されたユーザーはログインのための招待リンクを受け取ります：

<figure><img src="../.gitbook/assets/image (222).png" alt="" width="449"><figcaption></figcaption></figure>

ボタンをクリックすると、招待されたユーザーは直接FlowiseのSSOログイン画面に移動します：

<figure><img src="../.gitbook/assets/image (210).png" alt="" width="400"><figcaption></figcaption></figure>

またはFlowiseアプリに移動してSSOでサインインします：

<figure><img src="../.gitbook/assets/image (211).png" alt="" width="437"><figcaption></figcaption></figure>

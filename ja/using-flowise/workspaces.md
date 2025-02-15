# ワークスペース

{% hint style="info" %}
ワークスペースは現在エンタープライズ版でのみ利用可能です。Cloud Proプランでも近日提供予定です
{% endhint %}

初回ログイン時に、デフォルトのワークスペースが自動的に生成されます。ワークスペースは、様々なチームやビジネスユニット間でリソースを分割するために使用されます。各ワークスペース内では、ロールベースのアクセス制御(RBAC)を使用して権限とアクセスを管理し、ユーザーが自分の役割に必要なリソースと設定にのみアクセスできるようにします。

<figure><img src="../.gitbook/assets/Untitled-2024-10-19-0050.png" alt=""><figcaption></figcaption></figure>

## 管理者アカウントの設定

<details>

<summary>セルフホスト型エンタープライズの場合、以下の環境変数を設定する必要があります</summary>

```
JWT_AUTH_TOKEN_SECRET
JWT_REFRESH_TOKEN_SECRET
JWT_ISSUER
JWT_AUDIENCE
JWT_TOKEN_EXPIRY_IN_MINUTES
JWT_REFRESH_TOKEN_EXPIRY_IN_MINUTES
PASSWORD_RESET_TOKEN_EXPIRY_IN_MINS
PASSWORD_SALT_HASH_ROUNDS
TOKEN_HASH_SECRET
```

</details>

デフォルトでは、Flowiseの新規インストール時に管理者のセットアップが必要です。これはデータベースの初期設定時にrootユーザーを設定する必要があるのと同様です。

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt="" width="478"><figcaption></figcaption></figure>

設定後、ユーザーはFlowiseダッシュボードに移動します。左側のサイドバーには、ユーザー＆ワークスペース管理セクションが表示されます。デフォルトのワークスペースが自動的に作成されています。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## ワークスペースの作成

新しいワークスペースを作成するには、「Add New」をクリックします:

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

作成したワークスペースで、自分が組織管理者として追加されているのが確認できます。

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

ワークスペースに新しいユーザーを招待するには、まずロールを作成する必要があります。

## ロールの作成

左サイドバーの「Roles」に移動し、「Add Role」をクリックします:

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

ユーザーは各リソースに対して詳細な権限制御を指定できます。唯一の例外は、**User & Workspace Management**（Roles、Users、Workspaces、Login Activity）のリソースです。これらは現在アカウント管理者のみが利用可能です。

ここでは、すべてにアクセスできるエディターロールと、閲覧のみ可能な別のロールを作成します。

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

## ユーザーの招待

<details>

<summary>セルフホスト型エンタープライズの場合、以下の環境変数を設定する必要があります</summary>

```
INVITE_TOKEN_EXPIRY_IN_HOURS
SMTP_HOST
SMTP_PORT
SMTP_USER
SMTP_PASSWORD
```

</details>

左サイドバーの「Users」に移動すると、自分がアカウント管理者として表示されます。これは星付きの人物アイコンで示されます:

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

「Invite User」をクリックし、招待するメールアドレス、割り当てるワークスペース、およびロールを入力します。

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

「Send Invite」をクリックすると、招待されたメールアドレスに招待状が送信されます:

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

招待リンクをクリックすると、招待されたユーザーはサインアップページに移動します。

<figure><img src="../.gitbook/assets/image (10) (1).png" alt="" width="463"><figcaption></figcaption></figure>

招待されたユーザーがサインアップしてログインすると、割り当てられたワークスペースに入り、User & Workspace Managementセクションは表示されません:

<figure><img src="../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

複数のワークスペースに招待された場合、右上のドロップダウンボタンから異なるワークスペースに切り替えることができます。ここでは**閲覧のみ**の権限でWorkspace 2に割り当てられています。Chatflowの「Add New」ボタンが表示されなくなっていることに注目してください。これにより、ユーザーは閲覧のみ可能で、作成、更新、削除はできません。同じRBACルールはAPIにも適用されます。

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

アカウント管理者に戻ると、招待したユーザー、その状態、ロール、アクティブなワークスペースを確認できます:

<figure><img src="../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

アカウント管理者は他のユーザーの設定も変更できます:

<figure><img src="../.gitbook/assets/image (15) (1).png" alt=""><figcaption></figcaption></figure>

## ログイン履歴

管理者は全ユーザーのログインとログアウトの履歴を確認できます:

<figure><img src="../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

## ワークスペースでのアイテム作成

ワークスペースで作成されたアイテムは、他のワークスペースから分離されています。ワークスペースは、組織内のユーザーとリソースを論理的にグループ化する方法で、リソース管理とアクセス制御のための個別の信頼境界を確保します。各チームごとに個別のワークスペースを作成することをお勧めします。

ここでは、**Workspace1**で**Chatflow1**という名前のChatflowを作成します:

<figure><img src="../.gitbook/assets/image (16) (1).png" alt=""><figcaption></figcaption></figure>

**Workspace2**に切り替えると、**Chatflow1**は表示されません。これはAgentflows、Tools、Assistantsなどすべてのリソースに適用されます。

<figure><img src="../.gitbook/assets/image (17) (1).png" alt=""><figcaption></figcaption></figure>

以下の図は、組織、ワークスペース、およびワークスペースに関連付けられ含まれる様々なリソースの関係を示しています。

<figure><img src="../.gitbook/assets/Untitled-2024-10-19-0050.png" alt=""><figcaption></figcaption></figure>

## 認証情報の共有

認証情報を他のワークスペースと共有することができます。これにより、ユーザーは異なるワークスペースで同じ認証情報セットを再利用できます。

認証情報を作成した後、アカウント管理者またはRBACで認証情報共有の権限を持つユーザーは「Share」をクリックできます:

<figure><img src="../.gitbook/assets/image (18) (1).png" alt=""><figcaption></figcaption></figure>

ユーザーは認証情報を共有するワークスペースを選択できます:

<figure><img src="../.gitbook/assets/image (19) (1).png" alt=""><figcaption></figcaption></figure>

認証情報が共有されたワークスペースに切り替えると、共有された認証情報が表示されます。ユーザーは共有された認証情報を編集することはできません。

<figure><img src="../.gitbook/assets/image (20) (1).png" alt=""><figcaption></figcaption></figure>

## ワークスペースの削除

現在、ワークスペースの削除はアカウント管理者のみが実行できます。デフォルトでは、そのワークスペース内にまだユーザーが存在する場合、ワークスペースを削除することはできません。

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

まず、招待されたすべてのユーザーのリンクを解除する必要があります。これにより、特定のユーザーをワークスペースから削除したい場合の柔軟性が確保されます。なお、ワークスペースを作成した組織オーナーは、ワークスペースからリンクを解除することはできません。

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

招待されたユーザーのリンクを解除し、ワークスペース内に組織オーナーのみが残っている場合、削除ボタンがクリック可能になります:

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

ワークスペースの削除は元に戻せない操作であり、そのワークスペース内のすべてのアイテムがカスケード削除されます。警告ボックスが表示されます:

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

ワークスペースを削除すると、ユーザーはデフォルトワークスペースにフォールバックします。開始時に自動的に作成されたデフォルトワークスペースは削除できません。

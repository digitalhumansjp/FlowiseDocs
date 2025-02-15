---
description: FlowiseをAWSにデプロイする方法を学ぶ
---

# AWS

***

## 前提条件

AWSの基本的な仕組みを理解している必要があります。

FlowiseをAWSにデプロイするには2つのオプションがあります：

* [CloudFormationを使用してECSにデプロイ](aws.md#deploy-on-ecs-using-cloudformation)
* [EC2インスタンスを手動で設定](aws.md#launch-ec2-instance)

## CloudFormationを使用してECSにデプロイ

CloudFormationテンプレートはこちらで利用可能です: [https://gist.github.com/MrHertal/549b31a18e350b69c7200ae8d26ed691](https://gist.github.com/MrHertal/549b31a18e350b69c7200ae8d26ed691)

このテンプレートは、ELBを介して公開されるECSクラスターにFlowiseをデプロイします。

このテンプレートは以下のリファレンスアーキテクチャを参考にしています: [https://github.com/aws-samples/ecs-refarch-cloudformation](https://github.com/aws-samples/ecs-refarch-cloudformation)

Flowiseのイメージバージョンや環境変数などを調整するために、このテンプレートを自由に編集してください。

[AWS CLI](https://aws.amazon.com/fr/cli/)を使用してFlowiseをデプロイするコマンドの例:

```bash
aws cloudformation create-stack --stack-name flowise --template-body file://flowise-cloudformation.yml --capabilities CAPABILITY_IAM
```

デプロイ後、FlowiseアプリケーションのURLはCloudFormationスタックの出力で確認できます。

## Terraformを使用してECSにデプロイ

Terraformファイル（`variables.tf`、`main.tf`）は以下のGitHubリポジトリで利用可能です: [terraform-flowise-setup](https://github.com/huiseo/terraform-flowise-setup/tree/main)

このセットアップは、Application Load Balancer (ALB)を介して公開されるECSクラスターにFlowiseをデプロイします。これはAWSのECSデプロイメントのベストプラクティスに基づいています。

Terraformテンプレートを修正して以下を調整できます:

* Flowiseイメージバージョン
* 環境変数
* リソース設定（CPU、メモリなど）

### デプロイメントのコマンド例:

1. **Terraformの初期化:**

```bash
terraform init
terraform apply
terraform destroy
```

## EC2インスタンスの起動

1. EC2ダッシュボードで、**インスタンスを起動**をクリック

<figure><img src="../../.gitbook/assets/image (19) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. 下にスクロールし、キーペアがない場合は**新しいキーペアの作成**をクリック

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

3. 任意のキーペア名を入力します。Windowsの場合は`.ppk`を使用しPuTTYでインスタンスに接続します。MacとLinuxの場合は`.pem`を使用しOpenSSHで接続します

<figure><img src="../../.gitbook/assets/image (15) (2) (1).png" alt="" width="370"><figcaption></figcaption></figure>

4. **キーペアの作成**をクリックし、`.ppk`ファイルを保存する場所を選択
5. 左側のサイドバーを開き、**セキュリティグループ**から新しいタブを開きます。その後**セキュリティグループの作成**をクリック

<figure><img src="../../.gitbook/assets/image (20) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

6. 任意のセキュリティグループ名と説明を入力します。次に、インバウンドルールに以下を追加し**セキュリティグループを作成**をクリック

<figure><img src="../../.gitbook/assets/image (12) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

7. 最初のタブ（EC2インスタンスの起動）に戻り、**ネットワーク設定**までスクロールします。作成したセキュリティグループを選択

<figure><img src="../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

8. **インスタンスを起動**をクリック。EC2ダッシュボードに戻り、数分後に新しいインスタンスが起動して実行されているのが確認できます[🎉](https://emojipedia.org/party-popper/)

<figure><img src="../../.gitbook/assets/image (17) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## インスタンスへの接続方法（Windows）

1. Windowsの場合、PuTTYを使用します。[こちら](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)からダウンロードできます。
2. PuTTYを開き、**ホスト名**にインスタンスのパブリックIPv4 DNS名を入力します

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. PuTTY設定の左側のサイドバーから、**SSH**を展開し**Auth**をクリックします。参照をクリックし、先ほどダウンロードした`.ppk`ファイルを選択します。

<figure><img src="../../.gitbook/assets/image (23) (1) (1).png" alt="" width="296"><figcaption></figcaption></figure>

4. **開く**をクリックし、ポップアップメッセージを**承認**します

<figure><img src="../../.gitbook/assets/image (18) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

5. `ec2-user`としてログインします

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

6. これでEC2インスタンスに接続されました

## インスタンスへの接続方法（MacとLinux）

1. Mac/LinuxでTerminalアプリケーションを開きます。
2. _(オプション)_ プライベートキーファイルのアクセス権限を制限するために権限を設定します:

```bash
chmod 400 /path/to/mykey.pem
```

3. `ssh`コマンドを使用してEC2インスタンスに接続します。ユーザー名（`ec2-user`）、パブリックIPv4 DNS、`.pem`ファイルへのパスを指定します。

```bash
ssh -i /Users/username/Documents/mykey.pem ec2-user@ec2-123-45-678-910.compute-1.amazonaws.com
```

4. Enterを押すと、すべてが正しく設定されている場合、EC2インスタンスへのSSH接続が確立されます

## Dockerのインストール

1. yumコマンドを使用して保留中のアップデートを適用:

```bash
sudo yum update
```

2. Dockerパッケージを検索:

```bash
sudo yum search docker
```

3. バージョン情報を取得:

```bash
sudo yum info docker
```

4. Dockerをインストール:

```bash
sudo yum install docker
```

5. sudoコマンドを使用せずにすべてのdockerコマンドを実行できるように、デフォルトのec2-userにグループメンバーシップを追加:

```bash
sudo usermod -a -G docker ec2-user
id ec2-user
newgrp docker
```

6. docker-composeをインストール:

```bash
sudo yum install docker-compose-plugin
```

7. AMI起動時にdockerサービスを有効化:

```bash
sudo systemctl enable docker.service
```

8. Dockerサービスを開始:

```bash
sudo systemctl start docker.service
```

## Gitのインストール

```bash
sudo yum install git -y
```

## セットアップ

1. リポジトリをクローン

```bash
git clone https://github.com/FlowiseAI/Flowise.git
```

2. dockerフォルダに移動

```bash
cd Flowise && cd docker
```

3. `.env`ファイルを作成。お好みのエディタを使用できます。ここでは`nano`を使用

```bash
nano .env
```

<figure><img src="../../.gitbook/assets/image (13) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

4. 環境変数を指定:

```sh
PORT=3000
DATABASE_PATH=/root/.flowise
APIKEY_PATH=/root/.flowise
SECRETKEY_PATH=/root/.flowise
LOG_PATH=/root/.flowise/logs
BLOB_STORAGE_PATH=/root/.flowise/storage
```

5. _(オプション)_ アプリケーションレベルの認証のために`FLOWISE_USERNAME`と`FLOWISE_PASSWORD`も指定できます。詳細は[broken-reference](broken-reference/ "mention")を参照
6. `Ctrl + X`を押して終了し、`Y`を押してファイルを保存
7. docker composeを実行

```bash
docker compose up -d
```

7. アプリケーションがパブリックIPv4 DNSのポート3000で利用可能になります:

```
http://ec2-123-456-789.compute-1.amazonaws.com:3000
```

8. アプリケーションを停止するには:

```bash
docker compose stop
```

9. 最新のイメージを取得するには:

```bash
docker pull flowiseai/flowise
```

または:

```bash
docker-compose pull
docker-compose up --build -d
```

## NGINXを使用する

URLから:3000を削除してカスタムドメインを使用したい場合、NGINXを使用してポート80から3000へのリバースプロキシを設定できます。これにより、ユーザーは`http://yourdomain.com`のようなドメインでアプリにアクセスできます。

1. ```bash
   sudo yum install nginx
   ```
2. ```bash
   nginx -v
   ```
3. ```bash
   sudo systemctl start nginx
   ```
4. ```bash
   sudo nano /etc/nginx/conf.d/flowise.conf
   ```
5. 以下をコピーペーストし、ドメインを変更してください:

```shell
server {
    listen 80;
    listen [::]:80;
    server_name yourdomain.com; #例: demo.flowiseai.com
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

`Ctrl + X`を押して終了し、`Y`を押してファイルを保存

6. ```bash
   sudo systemctl restart nginx
   ```
7. DNSプロバイダーに移動し、新しいAレコードを追加します。名前はドメイン名、値はEC2インスタンスのパブリックIPv4アドレスになります

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt="" width="367"><figcaption></figcaption></figure>

6. これでアプリを開くことができます: `http://yourdomain.com`

### HTTPSを有効にするためのCertbotのインストール

アプリで`https://yourdomain.com`を使用したい場合の手順:

1. CertbotをインストールしNGINXでHTTPSを有効にするために、Pythonを使用します。まず、仮想環境をセットアップします:

```bash
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
```

2. 次に、このコマンドを実行してCertbotをインストールします:

```bash
sudo /opt/certbot/bin/pip install certbot certbot-nginx
```

3. `certbot`コマンドが実行できることを確認するために、次のコマンドを実行します:

```bash
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

4. 最後に、次のコマンドを実行して証明書を取得し、CertbotにNGINX設定を自動的に修正させてHTTPSを有効にします:

```bash
sudo certbot --nginx
```

5. 証明書生成ウィザードに従うと、`https://yourdomain.com`を使用してEC2インスタンスにHTTPSでアクセスできるようになります

## 自動更新の設定

Certbotが証明書を自動的に更新できるようにするには、以下のコマンドを実行してcronジョブを追加するだけです:

```bash
echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

## おめでとうございます！

SSL証明書を使用してドメイン上でEC2インスタンスにFlowiseアプリを正常にセットアップできました[🥳](https://emojipedia.org/partying-face/)

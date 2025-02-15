---
description: Digital Ocean での Flowise のデプロイ方法を学ぶ
---

# Digital Ocean

***

## Droplet の作成

このセクションでは、Droplet を作成します。詳細については、[公式ガイド](https://docs.digitalocean.com/products/droplets/quickstart/)を参照してください。

1. まず、ドロップダウンから **Droplets** をクリックします

<figure><img src="../../.gitbook/assets/image (15) (2).png" alt=""><figcaption></figcaption></figure>

2. データリージョンと Basic $6/月の Droplet タイプを選択します

<figure><img src="../../.gitbook/assets/image (17) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. 認証方法を選択します。この例では、パスワードを使用します

<figure><img src="../../.gitbook/assets/image (5) (2).png" alt=""><figcaption></figcaption></figure>

4. しばらくすると、Droplet が正常に作成されたことが確認できます

<figure><img src="../../.gitbook/assets/image (7) (2) (1).png" alt=""><figcaption></figcaption></figure>

## Droplet への接続方法

Windows の場合は、この[ガイド](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/putty/)に従ってください。

Mac/Linux の場合は、この[ガイド](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/openssh/)に従ってください。

## Docker のインストール

1. ```
   curl -fsSL https://get.docker.com -o get-docker.sh
   ```
2. ```
   sudo sh get-docker.sh
   ```
3. docker-compose のインストール：

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

4. 権限の設定：

```
sudo chmod +x /usr/local/bin/docker-compose
```

## セットアップ

1. リポジトリのクローン

```
git clone https://github.com/FlowiseAI/Flowise.git
```

2. docker フォルダに移動

```bash
cd Flowise && cd docker
```

3. `.env` ファイルの作成。お好みのエディタを使用できます。ここでは `nano` を使用します

```bash
nano .env
```

<figure><img src="../../.gitbook/assets/image (10) (2).png" alt="" width="375"><figcaption></figcaption></figure>

4. 環境変数の指定：

```sh
PORT=3000
DATABASE_PATH=/root/.flowise
APIKEY_PATH=/root/.flowise
SECRETKEY_PATH=/root/.flowise
LOG_PATH=/root/.flowise/logs
BLOB_STORAGE_PATH=/root/.flowise/storage
```

4. _(オプション)_ アプリケーションレベルの認証のために `FLOWISE_USERNAME` と `FLOWISE_PASSWORD` を指定することもできます。詳細は [broken-reference](broken-reference/ "mention") を参照してください
5. `Ctrl + X` を押して終了し、`Y` を押してファイルを保存します
6. docker compose の実行

```bash
docker compose up -d
```

7. "パブリック IPv4 DNS":3000 でアプリにアクセスできます。例：`176.63.19.226:3000`
8. 以下のコマンドでアプリを停止できます：

```bash
docker compose stop
```

9. 以下のコマンドで最新のイメージを取得できます：

```bash
docker pull flowiseai/flowise
```

## リバースプロキシと SSL の追加

リバースプロキシは、アプリケーションサーバーをインターネットに公開するための推奨される方法です。サーバーの IP とポート番号の代わりに URL だけで Droplet に接続することができます。これにより、アプリケーションサーバーを直接のインターネットアクセスから分離するセキュリティ上の利点、ファイアウォール保護の一元化、サービス拒否攻撃などの一般的な脅威に対する攻撃面の最小化、そして最も重要な目的である SSL/TLS 暗号化を一箇所で終端する機能が提供されます。

> Droplet に SSL がないと、最新のブラウザでは埋め込みウィジェットと API エンドポイントにアクセスできなくなります。これは、ブラウザが HTTP よりも HTTPS を優先するようになり、HTTPS で読み込まれたページからの HTTP リクエストをブロックするようになったためです。

### ステップ 1 — Nginx のインストール

1. Nginx はデフォルトのリポジトリから apt を使用してインストールできます。リポジトリインデックスを更新し、Nginx をインストールします：

```bash
sudo apt update
sudo apt install nginx
```

> Y を押してインストールを確認します。サービスの再起動を求められた場合は、ENTER を押してデフォルトを受け入れます。

2. サーバーの初期設定に従ってセットアップした後、ufw で以下のルールを追加してファイアウォールを通じて Nginx へのアクセスを許可する必要があります：

```bash
sudo ufw allow 'Nginx HTTP'
```

3. Nginx が実行されていることを確認できます：

```bash
systemctl status nginx
```

出力：

```bash
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2022-08-29 06:52:46 UTC; 39min ago
       Docs: man:nginx(8)
   Main PID: 9919 (nginx)
      Tasks: 2 (limit: 2327)
     Memory: 2.9M
        CPU: 50ms
     CGroup: /system.slice/nginx.service
             ├─9919 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             └─9920 "nginx: worker process
```

次に、ドメインとアプリケーションサーバーのプロキシを含むカスタムサーバーブロックを追加します。

### ステップ 2 — サーバーブロックと DNS レコードの設定

デフォルトの設定を直接編集する代わりに、新しいサーバーブロックの追加用にカスタム設定ファイルを作成することが推奨されます。

1. nano または任意のテキストエディタを使用して、新しい Nginx 設定ファイルを作成し開きます：

```bash
sudo nano /etc/nginx/sites-available/your_domain
```

2. 新しいファイルに以下を挿入し、`your_domain` を自分のドメイン名に置き換えてください：

```
server {
    listen 80;
    listen [::]:80;
    server_name your_domain; #例：demo.flowiseai.com
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_cache_bypass $http_upgrade;
    }
}
```

3. `nano` の場合は `CTRL+O` を押した後 `CTRL+X` を押して保存し終了します。
4. 次に、この設定ファイルを有効にするために、Nginx が起動時に読み込む sites-enabled ディレクトリにリンクを作成します。ここでも `your_domain` を自分のドメイン名に置き換えてください：

```bash
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

5. 設定ファイルの構文エラーをテストできます：

```bash
sudo nginx -t
```

6. 問題が報告されなければ、Nginx を再起動して変更を適用します：

```bash
sudo systemctl restart nginx
```

7. DNS プロバイダーに移動し、新しい A レコードを追加します。名前はドメイン名、値は Droplet のパブリック IPv4 アドレスになります

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt="" width="367"><figcaption></figcaption></figure>

これで Nginx がアプリケーションサーバーのリバースプロキシとして設定されました。http://yourdomain.com でアプリを開くことができるはずです。

### ステップ 3 — HTTPS (SSL) 用の Certbot のインストール

https://yourdomain.com のような安全な `https` 接続を Droplet に追加したい場合は、以下の手順を実行する必要があります：

1. NGINX に Certbot をインストールし HTTPS を有効にするために、Python を使用します。まず、仮想環境をセットアップします：

```bash
apt install python3.10-venv
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
```

2. その後、このコマンドを実行して Certbot をインストールします：

```bash
sudo /opt/certbot/bin/pip install certbot certbot-nginx
```

3. `certbot` コマンドが実行できることを確認するために、以下のコマンドを実行します：

```bash
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

4. 最後に、以下のコマンドを実行して証明書を取得し、Certbot に NGINX 設定を自動的に変更させ、HTTPS を有効にします：

```bash
sudo certbot --nginx
```

5. 証明書生成ウィザードに従った後、https://yourdomain.com のアドレスを使用して HTTPS 経由で Droplet にアクセスできるようになります

### 自動更新の設定

Certbot が証明書を自動的に更新できるようにするには、以下のコマンドを実行して cron ジョブを追加するだけで十分です：

```bash
echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

## おめでとうございます！

ドメインに SSL 証明書を設定し、Droplet に Flowise を正常にセットアップできました [🥳](https://emojipedia.org/partying-face/)

## Digital Ocean で Flowise を更新する手順

1. Flowise をインストールしたディレクトリに移動します

```bash
cd Flowise/docker
```

2. docker イメージを停止して削除します

注意：データベースは別のフォルダに保存されているため、これによってフローが削除されることはありません

```bash
sudo docker compose stop
sudo docker compose rm
```

3. 最新の Flowise イメージを取得します

最新のバージョンリリースは[こちら](https://github.com/FlowiseAI/Flowise/releases)で確認できます

```bash
docker pull flowiseai/flowise
```

4. docker を起動します

```bash
docker compose up -d
```

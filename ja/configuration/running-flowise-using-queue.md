# キューを使用したFlowiseの実行

デフォルトでは、FlowiseはNodeJSのメインスレッドで実行されます。しかし、大量の予測処理がある場合、これはうまくスケールしません。そのため、`main`（デフォルト）と`queue`の2つのモードを設定できます。

## キューモード

以下の環境変数を使用して、Flowiseを`queue`モードで実行できます。

<table><thead><tr><th width="263">変数</th><th>説明</th><th>タイプ</th><th>デフォルト</th></tr></thead><tbody><tr><td>MODE</td><td>Flowiseを実行するモード</td><td>列挙型文字列: <code>main</code>, <code>queue</code></td><td><code>main</code></td></tr><tr><td>WORKER_CONCURRENCY</td><td>ワーカーが並列処理できるジョブの数。1つのワーカーの場合、同時に処理できる予測タスクの数を意味します。詳細は<a href="https://docs.bullmq.io/guide/workers/concurrency">こちら</a></td><td>数値</td><td>10000</td></tr><tr><td>QUEUE_NAME</td><td>メッセージキューの名前</td><td>文字列</td><td>flowise-queue</td></tr><tr><td>QUEUE_REDIS_EVENT_STREAM_MAX_LEN</td><td>イベントストリームは自動的にトリミングされ、サイズが大きくなりすぎないようになっています。詳細は<a href="https://docs.bullmq.io/guide/events">こちら</a></td><td>数値</td><td>10000</td></tr><tr><td>REDIS_HOST</td><td>Redisホスト</td><td>文字列</td><td>localhost</td></tr><tr><td>REDIS_PORT</td><td>Redisポート</td><td>数値</td><td>6379</td></tr><tr><td>REDIS_USERNAME</td><td>Redisユーザー名（オプション）</td><td>文字列</td><td></td></tr><tr><td>REDIS_PASSWORD</td><td>Redisパスワード（オプション）</td><td>文字列</td><td></td></tr><tr><td>REDIS_TLS</td><td>Redis TLS接続（オプション）詳細は<a href="https://redis.io/docs/latest/operate/oss_and_stack/management/security/encryption/">こちら</a></td><td>真偽値</td><td>false</td></tr><tr><td>REDIS_CERT</td><td>Redis自己署名証明書</td><td>文字列</td><td></td></tr><tr><td>REDIS_KEY</td><td>Redis自己署名証明書キーファイル</td><td>文字列</td><td></td></tr><tr><td>REDIS_CA</td><td>Redis自己署名証明書CAファイル</td><td>文字列</td><td></td></tr></tbody></table>

`queue`モードでは、メインサーバーはリクエストの処理とメッセージキューへのジョブの送信を担当します。メインサーバーはジョブを実行しません。1つまたは複数のワーカーがキューからジョブを受け取り、実行して結果を返します。

これにより動的なスケーリングが可能になります：負荷が増加した時にワーカーを追加したり、負荷が軽い時期にワーカーを削除したりできます。

動作の仕組み：

1. メインサーバーがウェブから予測やその他のリクエストを受け取り、それらをキューにジョブとして追加します。
2. これらのジョブキューは、処理待ちのタスクのリストです。別のプロセスやスレッドであるワーカーがこれらのジョブを取得して実行します。
3. ジョブが完了すると、ワーカーは：
   * データベースに結果を書き込みます。
   * ジョブの完了を示すイベントを送信します。
4. メインサーバーがイベントを受け取り、UIに結果を返します。
5. Redis pub/subもUIへのデータストリーミングに使用されます。

<figure><img src="../.gitbook/assets/Untitled-2025-01-23-1520.png" alt=""><figcaption></figcaption></figure>

## Redisの起動

メインサーバーとワーカーを起動する前に、まずRedisを実行する必要があります。Redisは別のマシンで実行できますが、サーバーとワーカーのインスタンスからアクセス可能であることを確認してください。

例えば、この[ガイド](https://www.docker.com/blog/how-to-use-the-redis-docker-official-image/)に従ってDockerでRedisを実行できます。

## メインサーバーの設定

上記で説明した環境変数を設定する以外は、デフォルトのFlowiseの実行と同じです。

## ワーカーの設定

メインサーバーと同様に、上記の環境変数を設定する必要があります。メインとワーカーのインスタンス両方に同じ`.env`ファイルを使用することをお勧めします。唯一の違いはワーカーの実行方法です。

{% hint style="warning" %}
メインサーバーとワーカーは同じシークレットキーを共有する必要があります。[#for-credentials](environment-variables.md#for-credentials "mention")を参照してください。本番環境では、パフォーマンスのためにPostgresをデータベースとして使用することをお勧めします。
{% endhint %}

### NPMを使用したローカルでのFlowiseの実行

```bash
npx flowise worker # 環境変数の設定を忘れずに！
```

### Docker Compose

[ここ](https://github.com/FlowiseAI/Flowise/tree/main/docker/worker)で提供されている`docker-compose.yml`を使用するか、メインサーバー用に使用していた同じ`docker-compose.yml`を再利用し、エントリーポイントを`flowise start`から`flowise worker`に変更します：

```docker
version: '3.1'

services:
    flowise:
        image: flowiseai/flowise
        restart: always
        environment:
            - PORT=${PORT}
            ....
            - MODE=${MODE}
            - WORKER_CONCURRENCY=${WORKER_CONCURRENCY}
            ....
        ports:
            - '${PORT}:${PORT}'
        volumes:
            - ~/.flowise:/root/.flowise
        entrypoint: /bin/sh -c "sleep 3; flowise worker"
```

### Gitクローン

1つ目のターミナルでメインサーバーを実行

```bash
pnpm start
```

他のターミナルでワーカーを実行

```bash
pnpm start-worker
```

### AWS Terraform

_近日公開_

## キューダッシュボード

`<your-flowise-url.com>/admin/queues`にアクセスすることで、すべてのジョブ、それらのステータス、結果、データを表示できます。

<figure><img src="../.gitbook/assets/image (253).png" alt=""><figcaption></figcaption></figure>

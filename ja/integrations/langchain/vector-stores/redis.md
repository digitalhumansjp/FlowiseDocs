# Redis

## 前提条件

1. Dockerを使用して[Redis-Stack Server](https://redis.io/docs/latest/operate/oss_and_stack/install/install-stack/docker/)を起動

```bash
docker run -d --name redis-stack-server -p 6379:6379 redis/redis-stack-server:latest
```

## セットアップ

1. キャンバスに新しい**Redis**ノードを追加。
2. 新しいRedis認証情報を作成。

<figure><img src="../../../.gitbook/assets/image (1) (1) (3) (1) (1).png" alt="" width="257"><figcaption></figcaption></figure>

3. Redis認証情報のタイプを選択。ユーザー名とパスワードがある場合はRedis APIを、そうでない場合はRedis URLを選択:

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

4. URLを入力:

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (2) (1).png" alt="" width="542"><figcaption></figcaption></figure>

5. これでRedisでデータのアップサートを開始できます:

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (9) (2).png" alt=""><figcaption></figcaption></figure>

6. Redis Insightポータルに移動し、データベースでアップサートされたすべてのデータを確認できます:

<figure><img src="../../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

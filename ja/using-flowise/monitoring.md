# モニタリング

Flowiseは、PrometheusとGrafana、OpenTelemetryをネイティブにサポートしています。ただし、追跡されるのはAPIリクエスト、フロー/予測のカウントなどの高レベルのメトリクスのみです。カウンターメトリクスの一覧は[こちら](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/src/Interface.Metrics.ts#L13)を参照してください。ノード単位の詳細な可観測性については、[アナリティクス](analytic.md)の使用をお勧めします。

## Prometheus

[Prometheus](https://prometheus.io/)は、オープンソースの監視およびアラートソリューションです。

Prometheusを設定する前に、Flowiseで以下の環境変数を設定してください。

```properties
ENABLE_METRICS=true
METRICS_PROVIDER=prometheus
METRICS_INCLUDE_NODE_METRICS=true
```

Prometheusをインストールしたら、設定ファイルを使用して実行します。Flowiseは、[こちら](https://github.com/FlowiseAI/Flowise/blob/main/metrics/prometheus/prometheus.config.yml)にあるデフォルトの設定ファイルを提供しています。

Flowiseインスタンスも実行中であることを忘れないでください。ブラウザを開いてポート9090に移動します。ダッシュボードから、メトリクスエンドポイント - `/api/v1/metrics` が稼働していることが確認できるはずです。

<figure><img src="../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

デフォルトでは、`/api/v1/metrics` がPrometheusがメトリクスをプルするために利用可能になっています。

<figure><img src="../.gitbook/assets/image (177).png" alt="" width="563"><figcaption></figcaption></figure>

## Grafana

Prometheusは豊富なメトリクスを収集し、強力なクエリ言語を提供します。Grafanaはそれらのメトリクスを意味のある可視化に変換します。

Grafanaはさまざまな方法でインストールできます。[ガイド](https://grafana.com/docs/grafana/latest/setup-grafana/installation/)を参照してください。

Grafanaはデフォルトでポート9091を公開します:

<figure><img src="../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

左サイドバーで「Add new connection」をクリックし、Prometheusを選択します:

<figure><img src="../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

Prometheusはポート9090で動作しているため:

<figure><img src="../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

下までスクロールして接続をテストします:

<figure><img src="../.gitbook/assets/image (182).png" alt=""><figcaption></figcaption></figure>

ツールバーに表示されているデータソースIDをメモしておきます。これはダッシュボード作成時に必要になります:

<figure><img src="../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>

接続が正常に追加されたら、ダッシュボードの追加を開始できます。左サイドバーから「Dashboards」をクリックし、「Create Dashboard」を選択します。

Flowiseは2つのテンプレートダッシュボードを提供しています:

* [grafana.dashboard.app.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.app.json.txt): チャットフロー/エージェントフローの数、予測数、ツール、アシスタント、アップサートされたベクトルなどのAPIメトリクス
* [grafana.dashboard.server.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.server.json.txt): ヒープ、CPU、RAMの使用量などのFlowise Node.jsインスタンスのメトリクス

上記のテンプレートを使用する場合は、`cds4j1ybfuhogb`のすべての出現箇所を、先ほど作成して保存したデータソースIDに置き換えてください。

<figure><img src="../.gitbook/assets/image (183).png" alt=""><figcaption></figcaption></figure>

最初にインポートしてから後でJSONを編集することもできます:

<figure><img src="../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

Flowiseで何らかのアクションを実行すると、メトリクスが表示されるはずです:

<figure><img src="../.gitbook/assets/image (186).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (187).png" alt=""><figcaption></figcaption></figure>

## OpenTelemetry

[OpenTelemetry](https://opentelemetry.io/)は、テレメトリデータを作成および管理するためのオープンソースフレームワークです。OTelを有効にするには、Flowiseで以下の環境変数を設定します:

```properties
ENABLE_METRICS=true
METRICS_PROVIDER=open_telemetry
METRICS_INCLUDE_NODE_METRICS=true
METRICS_OPEN_TELEMETRY_METRIC_ENDPOINT=http://localhost:4318/v1/metrics
METRICS_OPEN_TELEMETRY_PROTOCOL=http # http | grpc | proto (デフォルトはhttp)
METRICS_OPEN_TELEMETRY_DEBUG=true
```

次に、テレメトリデータの受信、処理、エクスポートを行うためのOpenTelemetry Collectorが必要です。Flowiseは、コレクターコンテナを起動するために使用できる[docker composeファイル](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/compose.yaml)を提供しています。

```bash
cd Flowise
cd metrics && cd otel
docker compose up -d
```

コレクターは、同じディレクトリにある[otel.config.yml](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/otel.config.yml)ファイルを設定に使用します。現在は[Datadog](https://www.datadoghq.com/)とPrometheusのみがサポートされています。Zipkin、Jeager、New Relic、Splunkなどの異なるAPMツールを設定する場合は、[Open Telemetry](https://opentelemetry.io/)のドキュメントを参照してください。

ymlファイル内のエクスポーター用の必要なAPIキーを必ず置き換えてください。

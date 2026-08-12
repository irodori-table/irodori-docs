<!-- i18n: language-switcher -->
[English](README.md) | [日本語](README.ja.md)

# エンジンのチートシート

データベースごとに、**Irodoriからの接続方法、クエリモデル、エンジン固有の注意点**を素早く確認できるページです。詳しいドライバー／デコード仕様は[エンジン構文リファレンス](../engine-syntax-reference.md)、対応状況は[データソース対応状況](../data-source-support-status.md)を参照してください。

各チートシートは、`irodori-table`内のローカル知識入力（`knowledge/cheatsheets/*.json`と知識DB）から**自動生成**されることを意図しています。ページが生成されるまで、手動で種をまき`<!-- seed -->`とマークすることも可能です。生成管理とドリフトルールは[`repository-boundaries.md`](../repository-boundaries.md)に記録されています。

## 目次

| チートシート | 対応エンジン | 状況 |
|---|---|---|
| [neo4j.md](neo4j.md) | Neo4j（グラフ、Bolt/Cypher）；Memgraph拡張メモ | 種まき（グラフ/Boltの主要ページ） |
| [postgres.md](postgres.md) | PostgreSQL（+ Cockroach/Yugabyte/Redshift/Timescale/Neon；H2ワイヤーメモ） | 生成済み（`knowledge/cheatsheets/postgres.json`） |
| [questdb.md](questdb.md) | QuestDB（Postgresワイヤ＋時系列SQL拡張） | 種まき |
| _mysql.md_ | MySQL / MariaDB / TiDB | 計画中 |
| _sqlite.md_ | SQLite | 計画中 |
| _oracle.md_ | Oracle | 計画中 |
| _sqlserver.md_ | SQL Server | 計画中 |
| _duckdb.md_ | DuckDB / MotherDuck | 計画中 |
| _mongodb.md_ | MongoDB | 計画中 |
| _redis.md_ | Redis | 計画中 |
| _cassandra.md_ | Cassandra / ScyllaDB | 計画中 |
| _clickhouse.md_ | ClickHouse | 計画中 |
| _snowflake.md_ | Snowflake | 計画中 |
| _bigquery.md_ | BigQuery | 計画中 |
| _bigtable.md_ | Bigtable | 計画中 |
| _influxdb.md_ | InfluxDB | 計画中 |

新しいチートシートは、[データソース対応状況](../data-source-support-status.md)で少なくとも**Wired**と認定されたエンジンに追加します。「認識済み、拡張が必要」または「未登録」のエンジンは、実際に接続できるようになるまで対応状況ページだけで管理します。

## メンテナンスキュー

次のページは対応状況表の順序に合わせ、検証済みまたは接続済みのクエリパスと十分な公式情報があるエンジンを優先します：`duckdb.md`、`mongodb.md`、`redis.md`、`cassandra.md`、`clickhouse.md`、`snowflake.md`、`bigquery.md`、`bigtable.md`、`influxdb.md`。

## ページのフォーマット（各チートシートのテンプレート）

この順序を守ることで、生成ツールが決定論的にページを作成でき、読者も習慣化しやすくなります。

1. **概要** — ワイヤ/ドライバー、デフォルトポート、クエリ言語、Irodoriのサポート状況、そして「このエンジンの特徴は何か？」を一行で。
2. **接続** — Irodoriの接続フィールドと*生のURL/DSN形式*、最小限の動作例。
3. **クエリモデル** — 入力する内容、返ってくる内容、行制限の挙動。
4. **基本的なステートメント** — 80％のケースに対応できる、コンパクトで実行可能なクエリセット。
5. **オブジェクトの一覧表示** — Irodoriのオブジェクトブラウザのようにオブジェクトをリストアップする方法。
6. **Irodori固有の挙動** — このアプリ特有のマッピングや癖（デコード、オブジェクトブラウザのマッピング、未実装部分）。
7. **注意点** — 実際に人を困らせる小さなポイント。
8. **ソース** — このページが生成された`knowledge/sources.json`のID。

**Sources**フッターは重要で、各ページを公式ドキュメントに紐付け、ページの鮮度を確認できる仕組みになっています。

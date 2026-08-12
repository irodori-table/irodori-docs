# DB Feature Samples

[`db-feature-samples.json`](https://github.com/irodori-table/irodori-samples/blob/main/db-feature-samples.json)
in the sibling `irodori-samples` repository is the source of truth for
DB-specific sample projects, feature checks, and official learning resources.
It is intentionally machine-readable so the repository docs and public website
can stay aligned with the engine registry.

## Local sample projects

Use these when you want to check DB-specific behavior in Irodori Table beyond the
generic connection smoke test.

| Engine | Sample file | Verification command | Focus |
| --- | --- | --- | --- |
| PostgreSQL | `../irodori-samples/projects/postgres/queries.sql` | `task db-verify DB=postgres` | JSONB, arrays, GIN, extensions, explain JSON |
| MySQL | `../irodori-samples/projects/mysql/queries.sql` | `task db-verify DB=mysql` | JSON functions, FK metadata, windows, explain JSON |
| MariaDB | `../irodori-samples/projects/mariadb/queries.sql` | `task db-verify DB=mariadb` | JSON_VALID/JSON_VALUE, recursive CTEs, windows |
| SQLite | `../irodori-samples/projects/sqlite/queries.sql` | `cargo test --manifest-path apps/desktop/src-tauri/Cargo.toml sqlite` | Embedded SQL, JSON, FTS5, PRAGMA |
| DuckDB | `../irodori-samples/projects/duckdb/queries.sql` | `cargo test --manifest-path apps/desktop/src-tauri/Cargo.toml --features duckdb duckdb_in_memory` | Local analytics, structs, summarize, explain |
| SQL Server | `../irodori-samples/projects/sqlserver/queries.sql` | `task db-verify DB=sqlserver` | T-SQL JSON, identity, TOP, OFFSET/FETCH |
| Oracle | `../irodori-samples/projects/oracle/queries.sql` | `task db-verify DB=oracle` | Thin TNS, SQL/JSON, analytic functions, DBMS_XPLAN |
| MongoDB | `../irodori-samples/projects/mongodb/queries.js` | `task db-verify DB=mongodb` | Collection query, JSON filter, aggregation reference |
| TimescaleDB | `../irodori-samples/projects/timescaledb/queries.sql` | `task db-verify DB=timescaledb` | Hypertables, time_bucket, time-series metadata |
| CockroachDB | `../irodori-samples/projects/cockroachdb/queries.sql` | `task db-verify DB=cockroachdb` | unique_rowid, UPSERT, range inspection |
| YugabyteDB | `../irodori-samples/projects/yugabytedb/queries.sql` | `task db-verify DB=yugabytedb` | YSQL, split tablets, table properties |
| TiDB | `../irodori-samples/projects/tidb/queries.sql` | `task db-verify DB=tidb` | MySQL wire, tidb_version, shard_row_id_bits, explain analyze |

## Additional sample engines

The sibling sample repository now covers 23 engines with deterministic seed
data and per-engine query projects, including ClickHouse, Redis, Neo4j,
Memgraph, Cassandra, ScyllaDB, QuestDB, InfluxDB, Elasticsearch, OpenSearch,
Qdrant, ArangoDB, and DynamoDB. Some of these require an installable connector
extension rather than a built-in desktop driver. Cloud-only or hosted-only
targets such as Redshift, Neon, Snowflake, BigQuery, Bigtable, Milvus, and
Pinecone remain reference-only.

## Managed wire-compatible targets

These are not new low-level adapters. They should be surfaced as connection
templates/presets that route through existing engines.

| Target | Route through | Focus |
| --- | --- | --- |
| Supabase Postgres | `postgres` | Direct/pooler connection strings, SSL, RLS-aware browsing, hosted Postgres extensions such as pgvector |
| Amazon Aurora | `postgres` or `mysql` | Aurora PostgreSQL/MySQL endpoints, reader/writer endpoint guidance, IAM auth, cluster topology |
| Google Cloud SQL | `postgres`, `mysql`, or `sqlserver` | Public/private IP, Cloud SQL Auth Proxy, IAM DB auth, SSL certs, instance metadata |

## Lakehouse targets

Iceberg and S3 Tables are not SQL wire-compatible databases. They need a
catalog/table-format connection model plus an execution backend.

| Target | Route | Focus |
| --- | --- | --- |
| Apache Iceberg | REST/Hive/AWS Glue/JDBC catalogs + object store credentials | Catalog, namespace, table, schema, partition, snapshot, manifest, and metadata browsing |
| Amazon S3 Tables | Managed Iceberg table buckets | Table buckets, namespaces, tables, AWS IAM, and query execution through Athena/Redshift/Spark-compatible engines |

## Checks

Run the catalog guard directly:

```bash
node tools/docs/db-feature-samples.mjs
```

It is also part of:

```bash
task docs-check
```

The check fails when a registered engine has no catalog entry, a sample project
points at a missing file, a local compose sample is not represented, or an engine
has no official resource link.

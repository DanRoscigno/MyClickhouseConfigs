# MyClickhouseConfigs

## Apache Superset
I run Superset in Docker, the Dockerfile and compose.yml are provided.  The Dockerfile
includes the necessary driver for ClickHouse and sqlalchemy.  See the
[README](ApacheSuperset/README.md) for info on connecting from Superset to ClickHouse.

## Grafana Cloud
To connect Grafana Cloud to the public ClickHouse playground install the Grafana
ClickHouse plugin and see the [README](GrafanaCloud.md)

## Running the docs
node 18 seems to work fine, min is 14.13

There may be a `[WARNING] No docs found in zh: can't auto-generate a sidebar`, as there are no Chinese 
docs in the repo at the moment.

```
git clone git@github.com:ClickHouse/ClickHouse.git
git clone git@github.com:ClickHouse/clickhouse-docs.git
cd ClickHouse/docs/en
cp -r * ../../../clickhouse-docs/docs/en/
cd ../../../clickhouse-docs
yarn install
yarn run start
```

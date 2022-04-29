# MyClickhouseConfigs

## Apache Superset
I run Superset in Docker, the Dockerfile and compose.yml are provided.  The Dockerfile
includes the necessary driver for ClickHouse and sqlalchemy.  See the
[README](ApacheSuperset/README.md) for info on connecting from Superset to ClickHouse.

## Grafana Cloud
To connect Grafana Cloud to the public ClickHouse playground install the Grafana
ClickHouse plugin and see the [README](GrafanaCloud.md)

## Running the docs
```
git clone git@github.com:ClickHouse/ClickHouse.git
git clone git@github.com:ClickHouse/clickhouse-docs.git
cd clickhouse-docs/docs/en/
ln -s ../../../ClickHouse/docs/en/* .
cd ../../
mkdir -p ../ClickHouse/docs/en/images
cp ./docs/en/images/play.png ../ClickHouse/docs/en/images/
npm install
npx docusaurus start
```

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

### Run locally
Note: If you prefer to run in Docker see below

```
git clone git@github.com:ClickHouse/ClickHouse.git
git clone git@github.com:ClickHouse/clickhouse-docs.git

cp -r ClickHouse/docs/en/* clickhouse-docs/docs/en/
cp MyClickhouseConfigs/Docker/Dockerfile clickhouse-docs/

cd clickhouse-docs

yarn install
yarn run start
```
Launch http://localhost:3000/docs/en/intro 

### Dockerfile for docusaurus

The [Dockerfile](https://github.com/DanRoscigno/MyClickhouseConfigs/blob/main/Docker/Dockerfile) came from 
[Cindy Le](https://dev.to/cindyledev/how-to-dockerize-a-docusaurus-v2-application-fp7).
I popped it into the `clickhouse-docs` dir and followed the instructions in Cindy's post
to modify the docusaurus config.  I added a `chown` to the Dockerfile so that docusaurus (webpack)
would be able to [write cache files to `node_modules/`](https://webpack.js.org/configuration/cache/#cachecachedirectory).

Commands:
```
git clone git@github.com:ClickHouse/ClickHouse.git
git clone git@github.com:ClickHouse/clickhouse-docs.git
git clone git@github.com:DanRoscigno/MyClickhouseConfigs.git

cp -r ClickHouse/docs/en/* clickhouse-docs/docs/en/
cp MyClickhouseConfigs/Docker/Dockerfile clickhouse-docs/

cd clickhouse-docs
sed --in-place 's/"start": "docusaurus start",/"start": "docusaurus start --host 0.0.0.0",/' package.json
sed --in-place 's/"serve": "docusaurus serve",/"serve": "docusaurus serve --host 0.0.0.0",/' package.json
rm -rf package-lock.json node_modules
docker build --target development -t docs:dev .
docker run -p 3000:3000 docs:dev
```
Launch http://localhost:3000/docs/en/intro 

Note: The `sed` commands should be moved into the Dockerfile, no reason to modify the `package.json` outside of the 
Docker image.  Maybe move the `cp -r` commands into the Dockerfile also, the `docker build` can grab from the ClickHouse repo...

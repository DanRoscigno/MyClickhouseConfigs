The `docker-compose.yml` file in this dir sets up a replicated
ClickHouse system which uses ClickHouse Keeper for coordination.
This system is based on the 
[guide to creating unique ClickHouse Keeper entries](https://clickhouse.com/docs/en/guides/sre/keeper/clickhouse-keeper-uuid).

You can find the configuration files that will be used on the three containers in the `configs/` directory.  The files should match the snippets shown in the docs very closely.

```
docker compose up
```
There are three docker containers, for the most part in the docs it
is expected that you are connected to `chnode1`, so either run the 
`clickhouse-client` locally and connect to port 9001:
```
clickhouse-client  --port 9001
```

or run the client from `chnode1`:

```
docker compose exec chnode1 clickhouse-client
```

There is one step where you are told to insert into `chnode2`, so run
that on `chnode2` or connect to port 9002.

### Open a shell
```
docker compose exec chnode1 su clickhouse
```


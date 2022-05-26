## Run Superset in Docker
In this repo there is a `Dockerfile` and `docker-compose.yml`.  By default the Dockerfile will install all of the 
Superset demo data and visualizations.  You can just remove that line from the Dockerfile if you do not want that.

The plugins needed to connect to ClickHouse are installed during the build.

The admin password is also set in the Dockerfile, you can change the password in Superset.  I don't know if it will
persist if you change it.

Once the image is ready you will see a bunch of health check logs:
```
superset-superset-1  | 127.0.0.1 - - [26/May/2022:01:44:52 +0000] "GET /health HTTP/1.1" 200 2 "-" "curl/7.74.0"
superset-superset-1  | 127.0.0.1 - - [26/May/2022:01:45:22 +0000] "GET /health HTTP/1.1" 200 2 "-" "curl/7.74.0"
```

You can then connect to http://localhost:8080 and log in with `admin` and the password from the Dockerfile.

## Connecting Apache Superset to ClickHouse

If you are running ClickHouse on a Linux machine and Superset in Docker on 
that same machine, the IPADDR of the host machine, from the perspective of
your Apache Superset container, may be seen with
`ip a | grep docker`:

```
$ ip a|grep docker
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
```

Add some Covid19 data to ClickHouse by following step 3 of these
[instructions](https://clickhouse.com/learn/lessons/covidtutorial-superset/).

> **_NOTE:_**
The instructions linked to above give details for installing Superset
on your local machine, rather than in Docker. I prefer running this in Docker.


From above, 172.22.0.1 is the IPADDR of my host machine.  Since
ClickHouse is running on the host machine, I use `172.22.0.1` in
the Superset SQLALCHEMY URI:

```
clickhouse+native://default:supersecret@172.22.0.1/covid19db
```
Another example, connecting Preset's cloud offering to ClickHouse
```
clickhouse+native://default:supersecret@fully.qualified.domain.name:9440/default?secure=true
```

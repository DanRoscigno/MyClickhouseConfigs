If you are running Docker on Linux, the IPADDR of your host machine 
from the perspective of your Apache Superset container, may be seen with
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

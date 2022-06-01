## Cleanup
```
docker stop superset
docker rm superset
docker volume ls
docker volume rm supersetvol
```

## Setup

- Start the container with a persistent volume
```
docker run -d \
           --network="host" \
           --name superset \
           --mount source=supersetvol,target=/app \
           apache/superset:1.5.0
```

- Create the admin user
```
docker exec -it superset superset fab create-admin \
              --username admin \
              --firstname Superset \
              --lastname Admin \
              --email admin@superset.com \
              --password scoobydoo
```

- Upgrade the Superset database
```
docker exec -it superset superset db upgrade
```

- What is this?
```
docker exec -it superset superset init
```

- Add the drivers for ClickHouse and SQLAlchemy as root
```
docker exec -it -u 0 superset bash -c "pip install clickhouse-connect==0.0.10"
docker restart superset
```

- Open a browser and log in with the username / password for the admin user
```
http://admin:scoobydoo@localhost:8088
```

- Add a ClickHouse database
# clicking on Data and adding a database, choose ClickHouse Connect from the other supported and fill in the form:

  - Host: gf2e410ga9.us-east-2.aws.clickhouse-staging.com
  - Port: 8123
  - Username: default
  - Password: XXXXXXXX
  - Database: default
  - SSL: On

- Stop the container
In your browser log out of Superset and then stop the container:
```
docker stop superset
```

- Start the container again when needed
```
docker start superset
```


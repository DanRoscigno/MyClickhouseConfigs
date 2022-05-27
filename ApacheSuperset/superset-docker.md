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
docker run -d -p 8080:8088 \
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
docker exec -it -u 0 superset bash -c "pip install clickhouse-driver==0.2.3"
docker exec -it -u 0 superset bash -c "pip install clickhouse-sqlalchemy==0.1.9"
docker restart superset
```

- Open a browser and log in with the username / password for the admin user
```
http://admin:scoobydoo@localhost:8080
```

- Add a ClickHouse database
# clicking on Data and adding a database, choose clickhouse from the other supported and pasting in connection string 

clickhouse+native://default:FXarIEzaTYog@gf2e410ga9.us-east-2.aws.clickhouse-staging.com:9440/default?secure=true


- Stop the container
In your browser log out of Superset and then stop the container:
```
docker stop superset
```

- Start the container again when needed
```
docker start superset
```


FROM apache/superset
# Switching to root to install the required packages
USER root

# Example: installing the MySQL driver to connect to the metadata database
# if you prefer Postgres, you may want to use `psycopg2-binary` instead
#RUN pip install mysqlclient

# Find which driver you need based on the analytics database
# you want to connect to here:
# https://superset.apache.org/installation.html#database-dependencies
RUN pip install clickhouse-driver==0.2.0
RUN pip install clickhouse-sqlalchemy==0.1.6

# Switching back to using the `superset` user
USER superset

# Setup account
RUN superset fab create-admin \
               --username admin \
               --firstname Superset \
               --lastname Admin \
               --email admin@superset.com \
               --password scoobydoo

# Update the local DB
RUN superset db upgrade

# Load examples
RUN superset load_examples

# Setup roles
RUN superset init


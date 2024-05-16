---
{"dg-publish":true,"permalink":"/work/s4-msass-development/"}
---


# Notes for S4Msass system
## Install postgres with docker

```bash
# create & run Postgres docker version
docker run --name my-postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 -d postgres:15 


# docker exec to it & connect to DB
psql -U postgres -d postgres -h 127.0.0.1 -W

#Create database thingsboard;
create database thingsboard;


# Drop table if update sql
drop database thingsboard

# docker exec -it my-postgres psql -U postgres -d postgres -h 127.0.0.1 -c "CREATE DATABASE thingsboard;"
$ docker exec -it my-postgres psql -U postgres -h 127.0.0.1 -c "drop database thingsboard;"

# Backup & Restore DB in postgres
# postgres backup & restore
docker exec -t my-postgres pg_dump -c -U postgres db_name > dump.sql
cat your_dump.sql | docker exec -i your-db-container psql -U postgres db_name


```


## S4Msass run in Linux 
```bash
# Clone from source code , The latest develop version.
git clone -b devbranch https://app.suto-itec.asia:10443/s4m/s4msaas.git
# CD to s4msass path Build & Install 
mvn clean install -Dlicense.skip=true -DskipTests -e
cd ./application/target
dpkg -i thingsboard.deb

# Config DB
vim /usr/share/thingsboard/conf/thingsboard.conf

# Add to conf file
export DATABASE_ENTITIES_TYPE=sql
export DATABASE_TS_TYPE=sql
export SPRING_JPA_DATABASE_PLATFORM=org.hibernate.dialect.PostgreSQLDialect
export SPRING_DRIVER_CLASS_NAME=org.postgresql.Driver
export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/thingsboard
export SPRING_DATASOURCE_USERNAME=postgres
export SPRING_DATASOURCE_PASSWORD=postgres

# Run install script
cd /usr/share/thingsboard/bin/install
./install.sh --loadDemo
# or
/usr/share/thingsboard/bin/install/install.sh --loadDemo

# Start thingsboard service
service thingsboard start

# show system log 
tail /var/log/thingsboard/thingsboard.log -f

# Uninstall for update

dpkg -l | grep thingsboard
dpkg -r thingsboard
dpkg -P thingsboard
# Make sure clear 
dpkg -l | grep thingsboard


```

## S4Msass develop environment in Mac OS

### Maybe best enverionment

```
- java: 11.0.22 (Mac-OS using openjdk ) 
- maven: 3.6.3
- node: 18.19.1
- npm: 10.2.4
```

#### Run spring jar in different port

```
java -jar xxx.jar --server.port=8081
```

```bash

# NOT work
# mvn clean compile -Dlicense.skip=true

# For Installation
mvn clean install -Dlicense.skip=true -DskipTests -e
# or
mvn clean install -DskipTests -Dlicense.skip=true

# Using external  Postgres DB
# TODO

```json
{
    "version": "0.2.0",
    "configurations": [
      
      {
        "name": "Launch Chrome with ng serve",
        "type": "chrome",
        "request": "launch",
        "url": "http://localhost:4200",
        "webRoot": "${workspaceRoot}"
      },
      {
        "name": "Launch Chrome with ng test",
        "type": "chrome",
        "request": "launch",
        "url": "http://localhost:9876/debug.html",
        "webRoot": "${workspaceRoot}"
      },
      {
        "name": "Launch ng e2e",
        "type": "node",
        "request": "launch",
        "program": "${workspaceRoot}/node_modules/protractor/bin/protractor",
        "protocol": "inspector",
        "args": ["${workspaceRoot}/protractor.conf.js"]
      }      
    ]
  }

# copy /thingsboard/dao/src/main/resources/sql to 
#      /thingsboard/application/src/main/data/sql
# If it's the first time to init DB
cp -r dao/src/main/resources/sql application/src/main/data/
# also need cassandra if in hybrid mode
cp -r dao/src/main/resources/cassandra application/src/main/data

# or if in linux run time.
# cp -r dao/src/main/resources/sql /usr/share/thingsboard/data/
# cp -r dao/src/main/resources/cassandra /usr/share/thingsboard/data/


# run ThingsboardInstallApplication in IDE
# run ThingsboardServerApplication in IDE

# missing some sql script. 
docker cp application/src/main/data/sql/schema-ts-psql.sql my-postgres:/home/ts.sql
docker exec -u postgres my-postgres psql thingsboard postgres -f /home/ts.sql

# modify DB url if needed
vim application/src/main/resources/thingsboard.yml
datasource: 
  url: "${SPRING_DATASOURCE_URL:jdbc:postgresql://localhost:5432/thingsboard}"
  
# run UI
cd ui-ngx
yarn install
ng serve --open

```

### If only run frontend 

```

export NODE_OPTIONS=--openssl-legacy-provider
ng serve --proxy-config=proxy.conf.json --open
# run in hot load
ng serve --hmr

```



## Clear DB in S4M Saas

```bash
# Stop S4M application
service thingsboard stop
# Check port 8080 not in use
netstat -anp | grep 8080

# 
#go inside docker DB container
docker exec -it my-postgres bash
#login postgres
psql -U postgres -d postgres -h 127.0.0.1 -W
# drop thingsboard DB
drop database thingsboard;
# create empty one
create database thingsboard;
# init DB structure
/usr/share/thingsboard/bin/install/install.sh --loadDemo
# start S4M application
service thingsboard start
```


## S4M Saas Simulator

### S4M Saas Create Tenant

1. Login with system admin account
2. Create Tenant
3. Copy tenant number

### Simulator 

- Push device info to server (Exec simulator with 'Flag=0' )
   Enter tenant No (copy from website)
   Get accessToken back

- Send attribute to server (Exec simulator with 'Flag=1' & modify access token)

- Send telemetry (Exec simulator with 'Flag=2')




Note

-   **Admin**: sysadmin@thingsboard.org / sysadmin
-   **Tenant**: tenant@thingsboard.org / tenant
-   **Customer**: customer@thingsboard.org / customer


Some Opt

```bash
select * from attribute_kv where attribute_type = 'CLIENT_SCOPE';

```

#TODO/project
- using Kafka ?
- Virtual channel plan?


Test with cassendra
```bash
# cassandra in docker
docker run --rm -d --name mycassandra -p 19042:9042  cassandra
# create keyspace
docker exec -it cassandra cqlsh -e "CREATE KEYSPACE IF NOT EXISTS thingsboard WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };"



# config
export DATABASE_ENTITIES_TYPE=sql
export DATABASE_TS_TYPE=cassandra
export CASSANDRA_URL=127.0.0.1:19042
export SPRING_JPA_DATABASE_PLATFORM=org.hibernate.dialect.PostgreSQLDialect
export SPRING_DRIVER_CLASS_NAME=org.postgresql.Driver
export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:15432/thingsboard
export SPRING_DATASOURCE_USERNAME=postgres
export SPRING_DATASOURCE_PASSWORD=postgres




#docker status
$ docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}} "
CONTAINER ID   IMAGE         PORTS
6fa9fa705247   cassandra     7000-7001/tcp, 7199/tcp, 9160/tcp, 0.0.0.0:19042->9042/tcp, :::19042->9042/tcp
9510e2ff0270   postgres:15   0.0.0.0:15432->5432/tcp, :::15432->5432/tcp


# check cassandra
docker exec -it mycassandra bash
cqlsh
use thingsboard;
desc tables;
# or
docker exec -it mycassandra cqlsh -k thingsboard -e "desc tables";


# some opts
drop keyspace thingsboard;
docker exec -it mycassandra cqlsh -e "drop keyspace thingsboard"
select entity_type,ts, json_v from ts_kv_cf where entity_type='DEVICE' limit 10 allow filtering;


# result
cqlsh:thingsboard> desc tables;
ts_kv_cf  ts_kv_partitions_cf
cqlsh:thingsboard> select * from ts_kv_cf;
## Bugs?

```bash
# TODO

```


#s4m
#thingsboard

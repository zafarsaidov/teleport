[Main menu](../README.md)

---
# Install PostgeSQL for teleport

## PSQL

### Install docker

```bash
apt install docker.io
```

### Create Dockerfile for customizing psql

```bash
vim ~/Dockerfile
```

### Content of Dockerfile

```dockerfile
FROM postgres:16
RUN apt-get update
RUN apt-get install -y postgresql-16-wal2json
```

### Build docker image

```bash
docker build -t psql:16 ~
```

### Create unit file for psql

```bash
vim /etc/systemd/system/psql.service
```

### Content of unit file

```bash
[Unit]
Description=Docker Service for psql
After=network.service docker.service
Requires=docker.service

[Service]
ExecStartPre=-/usr/bin/docker stop -t 60 psql
ExecStartPre=-/usr/bin/docker rm psql
ExecStart=/usr/bin/docker run \
--rm \
--name psql \
-e POSTGRES_DB=postgres \
-e POSTGRES_USER=postgres \
-e POSTGRES_PASSWORD=postgres \
--publish 5432:5432 \
--volume pgsql_data:/var/lib/postgresql/data \
psql:16

ExecStop=-/usr/bin/docker stop -t 60 psql
ExecReload=-/usr/bin/docker restart psql

Restart=always
RestartSec=20s

SuccessExitStatus=SIGKILL SIGTERM 143 137

[Install]
WantedBy=multi-user.target
WantedBy=docker.service
```

### Start and enable psql service

```bash
systemctl daemon-reload
systemctl enable psql
systemctl start psql
systemctl status psql
```

### Command for connecting to psql

```bash
docker exec -it psql psql -U postgres
```

### SQL queries for creating teleport role and change wal level

```sql
CREATE ROLE teleport WITH PASSWORD 'teleport' LOGIN REPLICATION CREATEDB;
ALTER SYSTEM SET wal_level = 'logical';
```

---
[Main menu](../README.md)

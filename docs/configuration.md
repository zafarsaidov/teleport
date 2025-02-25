[Main menu](../README.md)

---
# Configuration teleport server

- [Teleport Configuration](https://goteleport.com/docs/reference/config/)


## Auth service

```bash
vim /etc/teleport.yaml
```

```yaml
auth_service:
  enabled: "yes"
  cluster_name: nameOfCluster
  listen_addr: 0.0.0.0:3025
  proxy_listener_mode: multiplex
```

## SSL

```bash
vim /etc/teleport.yaml
```

```yaml
proxy_service:
  enabled: "yes"
  web_listen_addr: :443
  public_addr: teleport.zafar.com:443
  https_keypairs: []
  acme:
    enabled: "yes"
    email: zafar@zafar.com
```

## Logs

```bash
vim /etc/teleport.yaml
```

```yaml
teleport:
  log:
    output: /var/lib/teleport/teleport.log
    severity: ERROR
    format:
      output: text
      extra_fields: [level, timestamp, component, caller]
```

## Connection limits

```bash
vim /etc/teleport.yaml
```

```yaml
teleport:
  connection_limits:
      max_connections: 1000
      max_users: 250
```

## Storage

```bash
mkdir ~/.aws
vim ~/.aws/credentials
```

```bash
[default]
aws_access_key_id = uuSiethae5jayiZuoCeego5phaeGa7oo
aws_secret_access_key = ahZ9nooqu1baTh7Queexai1ahraicieR
```

```bash
vim /etc/teleport.yaml
```

```yaml
teleport:
  storage:
    type: postgresql

    # conn_string: "postgresql://teleport:uyoihsdkmf@10.0.3.3:5432/teleport?sslcert=/root/certs/client.crt&sslkey=/root/certs/client.key&sslrootcert=/root/certs/ca.crt&sslmode=verify-full&pool_max_conns=20"
    
    conn_string: "postgresql://teleport:teleport@localhost:5432/teleport?sslmode=disable"

    audit_sessions_uri: "s3://myteleport?endpoint=http://localhost:9000&insecure=true&disablesse=true&region=uzb-1"
```


## Users

Add admin user

```bash
tctl users add teleport-admin --roles=editor,access --logins=root
```

[Manage users](https://goteleport.com/docs/admin-guides/management/admin/users/)



---
[Main menu](../README.md)

[Main menu](../README.md)

---
# Configuration teleport server

- [Teleport Configuration](https://goteleport.com/docs/reference/config/)


## SSL

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

```yaml
  log:
    output: /var/lib/teleport/teleport.log
    severity: ERROR
    format:
      output: text
      extra_fields: [level, timestamp, component, caller]
```

## Connection limits

```yaml
  connection_limits:
      max_connections: 1000
      max_users: 250
```

## Storage (psql + s3)

```yaml
  storage:
    type: postgresql

    # conn_string: "postgresql://teleport:uyoihsdkmf@10.0.3.3:5432/teleport?sslcert=/root/certs/client.crt&sslkey=/root/certs/client.key&sslrootcert=/root/certs/ca.crt&sslmode=verify-full&pool_max_conns=20"
    
    conn_string: "postgresql://teleport:uyoihsdkmf@10.0.3.3:5432/teleport?sslmode=disable"

    audit_sessions_uri: "s3://teleport-videos?endpoint=https:cdn.zafar.com"
```


## Users

Add admin user

```bash
tctl users add teleport-admin --roles=editor,access --logins=root
```


---
[Main menu](../README.md)

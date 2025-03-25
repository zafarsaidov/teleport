[Main menu](../README.md)

---
# Configure NGINX as a LB

## Install NGINX

```bash
apt update && apt install nginx
```

## Install certbot

```bash
apt install certbot
```

## Configuraion file content
```bash
server {
    server_name <teleport-domain>;
    access_log /var/log/nginx/teleport_access.log;
    error_log /var/log/nginx/teleport_error.log;

    location / {
        add_header       X-Served-By $host;
        proxy_set_header Host $host;
        proxy_set_header Connection 'Upgrade';
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header X-Forwarded-Scheme $scheme;
        proxy_set_header X-Forwarded-Proto  $scheme;
        proxy_set_header X-Forwarded-For    $remote_addr;
        proxy_set_header X-Real-IP          $remote_addr;
        proxy_pass       https://<teleport-server>:<proxy-service-port>;
    }
}
```

## Get SSL certificate via certbot

```bash
certbot --nginx
```


---
[Main menu](../README.md)

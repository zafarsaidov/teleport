
[Main menu](../README.md)

---

# Install Minio

## Create unit file
```bash
vim /etc/systemd/system/minio.service
```

## Content of unit file

```shell
[Unit]
Description=Docker Service for minio
After=network.service docker.service
Requires=docker.service

[Service]
ExecStartPre=-/usr/bin/docker stop -t 60 minio
ExecStartPre=-/usr/bin/docker rm minio
ExecStart=/usr/bin/docker run \
--rm \
--name minio \
--publish 9000:9000 \
--publish 9001:9001 \
-e "MINIO_ACCESS_KEY=uuSiethae5jayiZuoCeego5phaeGa7oo" \
-e "MINIO_SECRET_KEY=ahZ9nooqu1baTh7Queexai1ahraicieR" \
-v /miniodata:/data \
minio/minio:RELEASE.2024-01-05T22-17-24Z \
server /data --console-address :9001

ExecStop=-/usr/bin/docker stop -t 60 minio
ExecReload=/usr/bin/docker restart 'minio'

Restart=always
RestartSec=20s

SuccessExitStatus=SIGKILL SIGTERM 143 137

[Install]
WantedBy=multi-user.target
WantedBy=docker.service
```

## Start and enable service

```bash
systemctl daemon-reload
systemctl enable minio
systemctl start minio
systemctl status minio
```


---
[Main menu](../README.md)


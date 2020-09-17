# Reverse Proxy

Reverse-proxy based on nginx, with custom features:

- json logs
- geo-ip country restriction (only AR available)
- timezone America/Buenos_Aires
- automatic certbot renew

Example **docker-compose.yml** :

```yaml
version: "3.7"
services:
  reverse-proxy:
    image: maximilianoteruel/reverse-proxy:latest
    # use mode:host for passing real IP to container
    ports:
      - mode: host
        protocol: tcp
        published: 80
        target: 80
      - mode: host
        protocol: tcp
        published: 443
        target: 443
    volumes:
      - ./config-files/nginx.conf:/etc/nginx/conf.d/nginx.conf
      - nginx_log:/var/log/nginx
      - nginx_letsencrypt:/etc/letsencrypt
```

Initial **SSL** config:

```bash
$ docker exec -ti container_id sh
$ certbot
```

Example **server.conf** :

```nginx
server {
    server_name name;
    access_log /var/log/nginx/name_access.log json_combined;
    error_log /var/log/nginx/name_error.log;

    if ($allowed_country = no) {
            return 444;
        }

    # server configuration
}
```

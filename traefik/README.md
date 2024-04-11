# Traefik

Deploy [Traefik as reverse proxy](https://doc.traefik.io/traefik/getting-started/quick-start/) for your services in Docker.

It configures SSL certificates automatically using Let's Encrypt and Cloudflare.

## Setup

1. Clone the repository and navigate to the `traekif` directory.
2. Run `make init` to create the `.env` file and fill in the required environment variables.
3. Create a [Cloudflare API token](https://dash.cloudflare.com/profile/api-tokens) and fill in the `.env` file.
   Select the template `Edit zone DNS` > Select `doodo.shop` as Zone Resources > Set the IP address.

## Start Traefik

```shell
make up
```
or
```shell
docker-compose up -d
```

## Stop Traefik

```shell
make down
```
or
```shell
docker-compose down -d
```

## Expose a service

To expose a service, add the following **labels** to the service in the `docker-compose.yml` file. Make sure to attach 
the service to the `traefik` network:

```yaml
  web-app:
    image: nginx
    restart: unless-stopped
    expose:
      - 80
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.doodo-web-app.entrypoints=http,https"
      - "traefik.http.routers.doodo-web-app.rule=Host(`myhost.com`)"
      - "traefik.http.routers.doodo-web-app.tls=true"
      - "traefik.http.routers.doodo-web-app.tls.certresolver=cloudflare"
      - "traefik.http.routers.doodo-web-app.tls.domains[0].main=myhost.com"
    networks:
      - traefik
```

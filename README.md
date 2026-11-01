## 👋 Welcome to nginx 🚀

Nginx with PHP-FPM for serving PHP sites

## 📋 Description

Nginx with PHP-FPM for serving PHP sites. The image derives its document root
and virtual host from `BASE_HOST_NAME`, and mounts the matching host directory
from `/var/www/nginx/domains/`. Certificates from `/etc/letsencrypt` are
mounted so nginx can terminate TLS when configured.

## 🚀 Services

- **app**: richarvey/nginx-php-fpm:latest

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/nginx/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/nginx" ~/.local/srv/docker/nginx
cd ~/.local/srv/docker/nginx
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install nginx
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_ORG_NAME=nginx
```

`BASE_HOST_NAME` drives both the `DOMAIN` variable and the document root
bind mount, so it must match a directory under `/var/www/nginx/domains/`.

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64271` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64271

## 📂 Volumes

- `/etc/letsencrypt` - TLS certificates
- `/var/www/nginx/domains/${BASE_HOST_NAME}` - Document root

## 🔐 Security

- Configure HTTPS using a reverse proxy (nginx, traefik, caddy) for production
- The bind mounts are read-write; restrict their host permissions
- `GIT_USE_SSH: 1` lets the image run git over SSH, so keep any deploy keys in
  that directory restricted
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+
- A populated `/var/www/nginx/domains/${BASE_HOST_NAME}` directory on the host

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄

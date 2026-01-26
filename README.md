## 👋 Welcome to zulip 🚀

Open-source team chat with threaded conversations

## 📋 Description

Open-source team chat with threaded conversations

## 🚀 Services

- **zulip**: zulip/docker-zulip:latest

### Infrastructure Components

- **zulip-db**: Postgres database


## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/zulip/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/zulip" ~/.local/srv/docker/zulip
cd ~/.local/srv/docker/zulip
docker compose up -d
```

### Option 3: Using composemgr
```bash
composemgr install zulip
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
DB_USER_NAME=zulip
```

See `docker-compose.yaml` for complete list of configurable options.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:8093

## 📂 Volumes

- `./rootfs/data/zulip` - Data storage
- `./rootfs/config/zulip` - Data storage
- `./rootfs/data/db/postgres/zulip` - Data storage

## 🔐 Security

- Change all default passwords before deploying to production
- Use strong secrets for all authentication tokens
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose logs -f zulip
```

## 🛠️ Management

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# Update to latest images
docker compose pull && docker compose up -d

# View logs
docker compose logs -f

# Restart services
docker compose restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄

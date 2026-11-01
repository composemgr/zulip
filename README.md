## 👋 Welcome to Zulip 💬

Self-hosted team chat with topics, streams and email integration

## 📋 Description

Zulip is an open-source team chat and threaded discussion platform. Its
container image bundles the Django app, supervisor processes and the system
package setup, and it needs four backing services to run: PostgreSQL for the
data, Memcached for caches, Redis for rate limiting and RabbitMQ for the
background job queue. This stack runs all five on one internal network and
publishes only the web UI.

## 🚀 Services

- **app**: zulip/docker-zulip:latest

### Infrastructure Components

- **db**: zulip/zulip-postgresql:14, PostgreSQL database
- **redis**: redis:alpine
- **memcached**: memcached:alpine
- **rabbitmq**: rabbitmq:latest

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/zulip/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/zulip" ~/.local/srv/docker/zulip
cd ~/.local/srv/docker/zulip
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install zulip
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
DB_CREATE_DATABASE_NAME=zulip
DB_USER_NAME=zulip
DB_USER_PASS=replace-with-a-random-password
MEMCACHED_PASS=replace-with-a-random-password
REDIS_PASS=replace-with-a-random-password
APP_SECRET_KEY=replace-with-a-random-32-byte-hex-string
APP_ADMIN_USER=admin
EMAIL_SERVER_HOST=172.17.0.1
EMAIL_SERVER_PORT=587
EMAIL_SERVER_LOGIN_NAME=
EMAIL_SERVER_LOGIN_PASS=
EMAIL_SERVER_MAIL_FROM=noreply@example.com
```

`APP_ADMIN_USER` becomes `SETTING_ZULIP_ADMINISTRATOR` as
`<user>@<BASE_DOMAIN_NAME>`, so it must be an address the user can receive
mail at.

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
| `docker-compose.yaml` | Default, published on `172.17.0.1:64399` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

Only the `app` service carries the proxy labels and joins the external
network. The four datastores stay on the internal project network. Zulip
runs its own nginx inside the container, so websockets and long-polling
upgrade through port 80 without extra proxy configuration.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64399

## 📂 Volumes

- `./volumes/data/zulip` - Zulip `/data`, settings, secrets and uploaded files
- `./volumes/db/postgres/zulip` - Database data

## 🔐 Security

- Change `DB_USER_PASS`, `MEMCACHED_PASS` and `REDIS_PASS` before the first
  start, they are shared with the datastores
- Set `APP_SECRET_KEY` to a random 32 character or longer value, it signs
  sessions and API tokens
- `SETTING_REMOTE_POSTGRES_SSLMODE` is `require`, the database connection is
  encrypted inside the project network
- Set the `EMAIL_SERVER_LOGIN_*` variables, Zulip sends password reset and
  invitation mail from the host mail relay
- The datastores are reachable only on the internal project network
- Regularly update Docker images for security patches
- Back up `./volumes/data/zulip` and `./volumes/db/postgres/zulip` together

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
- At least 4 GB of memory, the first boot builds the Zulip Python environment
- Outbound SMTP on port 587 for invitations and password resets

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄

# OroCommerce Community Edition Docker Setup

This repository provides a complete Docker Compose configuration to run an OroCommerce instance locally or in a staging environment.  
It includes all required services and is fully compatible with Apple Silicon (ARM64).  
All configuration is managed through a single `.env` file.

---

## Services Included

- `web`: Nginx frontend for OroCommerce  
- `php-fpm-app`: PHP application (with healthcheck)  
- `ws`: WebSocket server  
- `consumer`: Message queue consumer  
- `cron`: Scheduled job runner  
- `application`: Main entrypoint that ensures dependencies are running  
- `install`: One-time OroCommerce installer  
- `restore`: Optional restore service  
- `init`: Prepares runtime configs (Nginx, PHP-FPM, etc.)  
- `db`: PostgreSQL with healthcheck  
- `mail`: Mailpit (SMTP + web UI for email testing)  

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/sebastianromero/orocommerce-docker.git
cd orocommerce-docker
```

### 2. Configure environment variables

Edit the `.env` file and adjust the values to match your setup:

- `ORO_IMAGE` / `ORO_IMAGE_TAG`: OroCommerce image and version  
- Admin user credentials  
- Mail configuration  
- Database credentials  
- App domain and protocol  
- `ORO_SAMPLE_DATA=y`: Enables demo sample data by default. Set to `n` to install without demo data.

For more information about installation parameters, see the official documentation:  
[InstallerBundle Commands – Oro Documentation](https://doc.oroinc.com/bundles/platform/InstallerBundle/commands/)

---

### 3. Launch the full stack

```bash
docker compose up -d
```

The app will be available at:

```
http://<your-app-domain>   # defined in ORO_APP_DOMAIN in .env
```

Mailpit (for testing emails) is available at:

```
http://localhost:8025
```

---

## Tips

- Data is persisted using Docker volumes.  
- To fully reset the stack:

```bash
docker compose down -v
```

- To reinstall OroCommerce from scratch:

```bash
docker compose down -v
docker compose up -d init db
docker compose run --rm install
```

---

## Apple Silicon Support

This setup runs natively on Apple Silicon (M1/M2/M3) using ARM64-compatible images.

---

## Environment Variable Reference

This project uses a `.env` file to configure all services. Below is a guide to the most important or less obvious variables.

### Installation Options

- **`ORO_SAMPLE_DATA`**  
  Controls whether to load demo data during installation.  
  - `y` (default): Includes sample products, categories, customer accounts, etc.  
  - `n`: Installs a clean OroCommerce instance without demo content.

- **`ORO_INSTALL_OPTIONS`**  
  Extra options passed directly to the `oro:install` command. Use this to override or extend default behavior.

[Full list of installer options](https://doc.oroinc.com/bundles/platform/InstallerBundle/commands/)

---

### Application Setup

- **`ORO_APP_PROTOCOL`**: Protocol used in URLs (`http` or `https`)  
- **`ORO_APP_DOMAIN`**: Application domain name  
- **`ORO_FORMATTING_CODE`**: Regional formatting (e.g., `en_US`, `es_CL`)  
- **`ORO_LANGUAGE`**: Default application language  

---

### Database Settings

- **`ORO_DB_DSN`**: Internal connection string (auto-generated)  
- **`ORO_DB_ROOT_USER` / `ORO_DB_ROOT_PASSWORD`**: Used for restore operations  

---

### Internal DSNs

- **`ORO_MQ_DSN`**: Message queue (default: `dbal:`)  
- **`ORO_SESSION_DSN`**: Session backend (default: `native:`)  
- **`ORO_SEARCH_ENGINE_DSN`**, **`ORO_WEBSITE_SEARCH_ENGINE_DSN`**: Search backend DSNs (default: ORM-based)  

[More about infrastructure-related config](https://doc.oroinc.com/backend/setup/dev-environment/parameters-yml/)

---

### WebSocket Configuration

- **`ORO_WEBSOCKET_BACKEND_HOST` / `PORT`**: Internal WebSocket host/port  
- **`ORO_WEBSOCKET_BACKEND_DSN`**: Backend service DSN  
- **`ORO_WEBSOCKET_SERVER_DSN`**: WebSocket server bind address  
- **`ORO_WEBSOCKET_FRONTEND_DSN`**: Frontend WebSocket connection URI  

---

## Mail Configuration

Configurar host como mail y el puerto 1025 sin ninguna autentificacion

---

## License

MIT License

```
Copyright (c) 2023 OroInc  
Copyright (c) 2025 Sebastián Romero
```

This project is based on the original OroInc Docker setup.  
Modified and maintained by Sebastián Romero under the same MIT License.

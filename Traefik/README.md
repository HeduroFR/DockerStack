# Traefik Stack 🚀

A complete Traefik reverse proxy setup with automatic HTTPS via Let's Encrypt, including Portainer for Docker management.

## 📦 What's Included

- **Traefik v3.5**: Modern reverse proxy with automatic SSL/TLS
- **Portainer CE**: Web-based Docker management interface
- **Let's Encrypt Integration**: Automatic SSL certificate generation and renewal
- **HTTP to HTTPS Redirect**: Automatic redirection for all traffic
- **Dashboard**: Built-in Traefik dashboard with authentication

## 🏗️ Architecture

```
Internet → Traefik (Port 80/443) → Docker Services
                ↓
           Portainer (Port 9000)
```

## 🚀 Quick Start

### 1. Create the Traefik Network

```bash
docker network create traefik
```

### 2. Configure Your Settings

#### Update Docker Compose

Edit `docker-compose.yml` and change:

```yaml
- "--certificatesresolvers.letsencryptresolver.acme.email=letsencrypt@example.com"
```

Replace `letsencrypt@example.com` with your actual email address.

Update Portainer hostname:

```yaml
- "traefik.http.routers.portainer.rule=Host(`portainer.example.com`)"
```

Replace `portainer.example.com` with your actual domain.

#### Update Configuration File

Edit `config.yaml` to add your services. See the example configuration for:
- Custom routers
- Middleware (CORS, BasicAuth, etc.)
- TLS settings

### 3. Prepare Certificate Storage

```bash
mkdir -p letsencrypt
touch letsencrypt/acme.json
chmod 600 letsencrypt/acme.json
```

### 4. Generate Basic Auth Credentials (Optional)

For dashboard authentication:

```bash
# Install htpasswd if not available
sudo apt-get install apache2-utils

# Generate credentials
htpasswd -nb username password
```

Update the credentials in `config.yaml` under `middleware-basic-auth`.

### 5. Deploy the Stack

```bash
docker compose up -d
```

### 6. Verify Deployment

```bash
docker compose ps
docker compose logs -f
```

## 📁 File Structure

```
Traefik/
├── README.md                # This file
├── docker-compose.yml       # Main compose configuration
├── config.yaml              # Traefik dynamic configuration
├── users.u                  # User credentials file
└── letsencrypt/             # Let's Encrypt certificates
    └── acme.json           # Certificate storage (auto-generated)
```

## ⚙️ Configuration Details

### Entrypoints

- **web (Port 80)**: HTTP traffic, auto-redirects to HTTPS
- **websecure (Port 443)**: HTTPS traffic with TLS 1.3
- **ssh (Port 22)**: Optional SSH proxy

### Middlewares

#### Basic Authentication
Protects services with username/password:
```yaml
middleware-basic-auth:
  basicAuth:
    users:
      - "username:hashed-password"
```

#### CORS Headers
Enables cross-origin requests:
```yaml
middleware-cors:
  headers:
    accessControlAllowOriginList:
      - "*"
```

#### HTTPS Redirect
Forces HTTPS for all connections:
```yaml
redirect-to-https:
  redirectScheme:
    scheme: https
    permanent: true
```

### TLS Configuration

- Minimum TLS version: 1.3
- SNI Strict mode enabled
- Automatic certificate resolution via Let's Encrypt

## 🔧 Adding New Services

To add a new service to Traefik, add labels to your service in its `docker-compose.yml`:

```yaml
services:
  myapp:
    image: myapp:latest
    networks:
      - traefik
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.myapp.rule=Host(`myapp.example.com`)"
      - "traefik.http.routers.myapp.entrypoints=websecure"
      - "traefik.http.routers.myapp.tls.certresolver=letsencryptresolver"
      - "traefik.http.services.myapp.loadbalancer.server.port=8080"

networks:
  traefik:
    external: true
```

## 🛠️ Advanced Configuration

### Using File Provider

Edit `config.yaml` to add custom routers and services:

```yaml
http:
  routers:
    my-router:
      service: my-service
      rule: "Host(`app.example.com`)"
      tls:
        certResolver: "letsencryptresolver"
      middlewares:
        - middleware-cors

  services:
    my-service:
      loadBalancer:
        servers:
          - url: "http://192.168.1.100:8080"
```

### Enable Debug Logging

Uncomment in `docker-compose.yml`:

```yaml
- "--log.level=DEBUG"
```

### Staging Certificates

For testing, use Let's Encrypt staging server (uncomment in `docker-compose.yml`):

```yaml
- "--certificatesresolvers.letsencryptresolver.acme.caserver=https://acme-staging-v02.api.letsencrypt.org/directory"
```

## 📊 Accessing Services

- **Portainer**: `https://portainer.example.com`
- **Traefik Dashboard**: Configure via labels (requires authentication)

## 🔐 Security Best Practices

1. **Change Default Credentials**: Update all passwords in `config.yaml`
2. **Restrict Dashboard Access**: Use basic auth or IP whitelisting
3. **Regular Updates**: Keep Traefik and Docker images updated
4. **Secure acme.json**: Ensure proper permissions (600)
5. **Use Strong Passwords**: Generate strong passwords for all services
6. **Monitor Logs**: Regularly check for suspicious activity

## 🐛 Troubleshooting

### Certificate Issues

Check certificate status:
```bash
docker compose logs traefik | grep -i certificate
cat letsencrypt/acme.json
```

### Connection Refused

Verify service is in traefik network:
```bash
docker network inspect traefik
```

### Rate Limiting

Let's Encrypt has rate limits. Use staging server for testing:
- [Let's Encrypt Rate Limits](https://letsencrypt.org/docs/rate-limits/)

## 📚 Resources

- [Traefik Documentation](https://doc.traefik.io/traefik/)
- [Docker Provider](https://doc.traefik.io/traefik/providers/docker/)
- [Let's Encrypt](https://letsencrypt.org/)
- [Portainer Documentation](https://docs.portainer.io/)

## 🔄 Maintenance

### Update Images

```bash
docker compose pull
docker compose up -d
```

### Backup

Important files to backup:
- `docker-compose.yml`
- `config.yaml`
- `letsencrypt/acme.json`
- Portainer data: `/data/portainer-data`

### Logs

View logs:
```bash
docker compose logs -f traefik
docker compose logs -f portainer
```

## ⚠️ Notes

- First certificate generation may take a few minutes
- Ensure DNS records point to your server before deployment
- Let's Encrypt requires port 80 to be accessible for HTTP challenge
- Rate limits apply: max 5 certificates per week per domain

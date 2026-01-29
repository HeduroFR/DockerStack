# DockerStack 🐳

A curated collection of production-ready Docker Compose stacks for self-hosting various services. Each stack is designed to be plug-and-play with minimal configuration required.

## 📋 Overview

This repository contains Docker Compose configurations for deploying various services quickly and efficiently. All stacks are designed with best practices in mind, including:

- **Security**: TLS/SSL support with Let's Encrypt
- **Reverse Proxy**: Traefik integration for automatic routing
- **Persistence**: Proper volume management
- **Networking**: Isolated networks for each stack
- **Production-Ready**: Restart policies and health checks

## 🚀 Available Stacks

### [Traefik](./Traefik) - Reverse Proxy & Load Balancer
A modern HTTP reverse proxy and load balancer with automatic Let's Encrypt SSL certificates.

**Includes:**
- Traefik v3.5 with automatic HTTPS
- Portainer for Docker management
- Dashboard with authentication
- Let's Encrypt certificate resolver

## 📚 Getting Started

### Prerequisites

- Docker Engine 20.10+
- Docker Compose 2.0+
- A domain name (for SSL certificates)

### Quick Start

1. **Clone the repository:**
   ```bash
   git clone https://github.com/HeduroFR/DockerStack.git
   cd DockerStack
   ```

2. **Choose a stack:**
   ```bash
   cd <stack-name>
   ```

3. **Configure the stack:**
   - Copy and edit configuration files
   - Update domain names
   - Set email addresses for Let's Encrypt
   - Configure authentication credentials

4. **Deploy:**
   ```bash
   docker compose up -d
   ```

## 🔧 Configuration

Each stack contains its own README with specific configuration instructions. General guidelines:

- **Environment Variables**: Check `.env.example` files
- **Volumes**: Data persists in mounted volumes
- **Networks**: Each stack uses isolated networks
- **Ports**: Modify exposed ports as needed

## 📖 Documentation Structure

```
DockerStack/
├── README.md                    # Main documentation (this file)
├── <Stack-Name>/
│   ├── README.md               # Stack-specific documentation
│   ├── docker-compose.yml      # Main compose file
│   ├── config.yaml             # Stack configuration
│   └── ...                     # Additional files
```

## 🤝 Contributing

Contributions are welcome! If you have a useful Docker stack to share:

1. Fork the repository
2. Create a new directory for your stack
3. Include a comprehensive README
4. Submit a pull request

## 📝 Roadmap

This repository will continue to grow with additional stacks. Planned additions include:

- Monitoring & Logging stacks
- Database stacks
- Application servers
- Media servers
- Backup solutions
- And more...

## ⚠️ Security Notes

- Always change default passwords and credentials
- Use strong passwords for all services
- Keep your Docker images updated
- Review configurations before deploying to production
- Use secrets management for sensitive data

## 📄 License

This repository is provided as-is. Individual stacks may have their own licenses based on the software they deploy.

## 💬 Support

For issues or questions:
- Open an issue on GitHub
- Check individual stack README files
- Review the official documentation of each service

## 🔗 Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Traefik Documentation](https://doc.traefik.io/traefik/)

---

**Note**: This is an actively maintained repository. New stacks will be added over time. Star ⭐ the repository to stay updated!

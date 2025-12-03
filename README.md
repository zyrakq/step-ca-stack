# 🐳 Step CA Docker Stack

This project contains Docker configurations and compose files for running Smallstep Step CA (Certificate Authority) and related services with web-based management interface.

## 🧩 Components

### [🔐 Step CA Certificate Authority](src/step-ca)

Smallstep Step CA — a private Certificate Authority for issuing and managing X.509 certificates. Provides automated certificate management through CLI and API interfaces.

[Learn more about Step CA configuration](src/step-ca/README.md).

### [🖥️ Step UI Web Interface](src/webui/step-ui)

Step UI — a web-based management interface for Smallstep Step CA. Provides an intuitive dashboard for certificate lifecycle management, monitoring provisioners, and administration tasks.

[Learn more about Step UI configuration](src/webui/step-ui/README.md).

## 🚀 Getting Started

To run the services, use the appropriate `docker-compose.yml` files in the subprojects. Make sure all environment variables are configured correctly.

Each service directory contains:

- 📋 Docker Compose configurations
- 🔧 Environment variable examples
- 📖 Detailed setup instructions
- 🛠️ Helper scripts for development and production

## 🏗️ Project Structure

```sh
├── src/
│   ├── step-ca/              # Step CA Certificate Authority
│   └── webui/               # Web-based management interfaces
│       └── step-ui/         # Step UI web interface (submodule)
```

## 📄 License

This project is dual-licensed under:

- [Apache License 2.0](LICENSE-APACHE)
- [MIT License](LICENSE-MIT)

# 🐳 Step CA Docker Stack

This project contains Docker configurations and compose files for running Smallstep Step CA (Certificate Authority) and related services.

## 🧩 Components

### [🔐 Step CA Certificate Authority](src/step-ca)

Smallstep Step CA — a private Certificate Authority for issuing and managing X.509 certificates. Provides automated certificate management through CLI and API interfaces.

[Learn more about Step CA configuration](src/step-ca/README.md).

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
│   └── step-ca/              # Step CA Certificate Authority
│       ├── components/       # Source Docker Compose components
│       │   ├── base/        # Core Step CA service
│       │   └── environments/ # Environment configurations
│       └── build/           # Generated configurations (via stackbuilder)
│           ├── devcontainer/ # VS Code Dev Containers environment
│           ├── forwarding/   # Local access with port forwarding
│           └── internal/     # Internal Docker network deployment
```

## 🔮 Future Plans

This project will be extended with GUI management interfaces as submodules to provide web-based certificate management capabilities.

## 📄 License

This project is dual-licensed under:

- [Apache License 2.0](LICENSE-APACHE)
- [MIT License](LICENSE-MIT)

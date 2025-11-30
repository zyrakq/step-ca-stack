# 🔐 Step CA Certificate Authority

A modular Docker Compose configuration system for [Smallstep Step CA](https://github.com/smallstep/certificates) with support for multiple deployment environments.

## 🚀 Quick Start

### 1. Build Configurations

Generate all configurations using [stackbuilder](https://github.com/zyrakq/stackbuilder):

```bash
sb build
```

This creates ready-to-use Docker Compose configurations for Step CA in the `build/` directory.

### 2. Deploy

Navigate to your chosen configuration and deploy:

```bash
# Example: deploy with port forwarding for local access
cd build/forwarding/
cp .env.example .env
# Edit .env with your values (especially DOCKER_STEPCA_INIT_PASSWORD)
docker compose up --build -d
```

For more information about Step CA configuration, visit the [official Smallstep documentation](https://smallstep.com/docs/step-ca).

## 📁 Project Structure

- **`components/`** - Source Docker Compose components
  - `base/` - Core Step CA service configuration
  - `environments/` - Environment configurations (devcontainer, forwarding, internal)
- **`build/`** - Generated configurations (created by `sb build`)
- **`stackbuilder.toml`** - Build configuration for stackbuilder

## 🔧 Available Configurations

### Environments

- **devcontainer** - Development environment with external workspace network for VS Code Dev Containers
- **forwarding** - Local access configuration with port 9000 exposed to host
- **internal** - Internal Docker network deployment without port forwarding (base configuration)

Generated configurations are available in the `build/` directory after running `sb build`.

## 🔧 Environment Variables

### Base Configuration (from `components/base/.env.example`)

**Project Settings:**

- `COMPOSE_PROJECT_NAME`: Project name for Docker Compose (default: `step-ca`)

**Step CA Initialization:**

- `DOCKER_STEPCA_INIT_NAME`: Name of the Certificate Authority (default: `"Step CA"`)
- `DOCKER_STEPCA_INIT_DNS_NAMES`: Comma-separated list of DNS names for CA certificate (default: `localhost,step-ca-management,127.0.0.1`)
- `DOCKER_STEPCA_INIT_PASSWORD`: Password for CA initialization and operations (required, generate a strong password)
- `DOCKER_STEPCA_INIT_PASSWORD_FILE`: Path to password file inside container (default: `/run/secrets/passwd`)

### Environment-Specific Configuration

**Internal Environment:**

- Uses base configuration only
- No port forwarding
- Accessible only within Docker network at `https://step-ca-management:9000`

**Forwarding Environment:**

- Same as internal configuration
- Exposes port `9000:9000` for local access
- Accessible at `https://localhost:9000`

**Devcontainer Environment:**

- Same as internal configuration
- Connects to external workspace network `step-ca-managmen-workspace-network`
- Suitable for VS Code Dev Containers integration

## 🔌 Service Details

### Step CA Management

- **Container Name**: `step-ca-management`
- **Image**: `smallstep/step-ca:latest`
- **Port**: 9000 (HTTPS API)
- **Health Check**: `step ca health --ca-url=https://localhost:9000`
- **Volume**: `step-ca-management-data` (persists CA configuration and certificates)
- **Network**: `step-ca-management-network`

### Accessing the CA

After deployment, you can interact with Step CA using the `step` CLI:

```bash
# Bootstrap trust with the CA (first time only)
step ca bootstrap --ca-url https://localhost:9000 --fingerprint <CA_FINGERPRINT>

# Request a certificate
step ca certificate example.com example.crt example.key

# Get CA fingerprint
docker exec step-ca-management step certificate fingerprint /home/step/certs/root_ca.crt
```

## 🛠️ Development

### Adding New Components

1. **New Environment**: Create directory in `components/environments/` with `docker-compose.yml` and optional `.env.example`
2. **Update Configuration**: Modify [`stackbuilder.toml`](stackbuilder.toml) to include new environment
3. **Rebuild**: Run `sb build` to regenerate configurations

### Modifying Components

1. Edit files in `components/`
2. Run `sb build` to regenerate all configurations
3. The `build/` directory will be completely recreated

## 📝 Notes

- The `build/` directory is automatically generated - do not edit manually
- User `.env` files are preserved during rebuilds
- All configurations are built from [`stackbuilder.toml`](stackbuilder.toml) specification
- Generate a strong random password for `DOCKER_STEPCA_INIT_PASSWORD` before deployment
- CA data is persisted in Docker volume `step-ca-management-data`
- The CA is initialized automatically on first run using the provided environment variables

## 🔗 Source Code

The original Step CA project source code is available at: [https://github.com/smallstep/certificates](https://github.com/smallstep/certificates)

## 📚 Additional Resources

- [Step CA Documentation](https://smallstep.com/docs/step-ca)
- [Step CLI Documentation](https://smallstep.com/docs/step-cli)
- [Certificate Management Best Practices](https://smallstep.com/docs/tutorials)

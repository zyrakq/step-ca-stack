# 🔧 Step CLI Reference Guide

A comprehensive reference guide for working with Step CA using the command-line interface.

> **Note:** The CLI tool is commonly referred to as `step` in most documentation and installations, but in this guide we use `step-cli` for clarity. Both commands work identically - just replace `step-cli` with `step` if that's how it's installed on your system.

## 📦 Installation

To install the Step CLI tool, follow the official installation guide:

**Installation Documentation:** [https://smallstep.com/docs/step-cli/installation/](https://smallstep.com/docs/step-cli/installation/)

---

## 🚀 Common Operations

### 🔗 Bootstrap and Add Existing Context

Bootstrap trust with your Step CA server and add it to your local configuration:

```sh
step-cli ca bootstrap --ca-url=https://localhost:9000 --fingerprint=<fingerprint> --install -f --context=dev
```

**Parameters:**

- `--ca-url` - URL of your Step CA server
- `--fingerprint` - CA root certificate fingerprint (obtain using the command below)
- `--install` - Install the root certificate in your system's trust store
- `-f` - Force overwrite if context already exists
- `--context` - Name for this CA context (e.g., `dev`, `prod`, `staging`)

---

### 📋 List Provisioners

View all available provisioners configured in your Step CA:

```sh
step-cli ca provisioner list
```

Provisioners are entities authorized to issue certificates. This command displays all configured provisioners and their settings.

---

### ⏱️ Update Certificate Validity Duration

Modify the minimum, maximum, and default validity periods for certificates issued by a provisioner:

```sh
step-cli ca provisioner update <name> --x509-min-dur 720h --x509-max-dur 8784h --x509-default-dur 720h
```

**Parameters:**

- `<name>` - Name of the provisioner to update
- `--x509-min-dur` - Minimum certificate validity duration (e.g., `720h` = 30 days)
- `--x509-max-dur` - Maximum certificate validity duration (e.g., `8784h` = 366 days)
- `--x509-default-dur` - Default certificate validity duration (e.g., `720h` = 30 days)

**Duration Format:** Use hours (`h`), minutes (`m`), or seconds (`s`). Examples: `24h`, `720h`, `8760h`

---

### 📥 Copy Root Certificate from Server

Download the root CA certificate from the server to your local machine:

```sh
step-cli ca root $(step-cli path)/certs/root_ca.crt
```

This command fetches the root certificate and saves it to your Step CLI configuration directory.

---

### 🔑 Copy Root Certificate Private Key

Copy the root CA private key from the Docker container to your local machine:

```sh
docker cp step-ca-management:/home/step/secrets/root_ca_key $(step-cli path)/secrets/root_ca_key
```

**⚠️ Security Warning:** The root CA private key is highly sensitive. Only copy it when absolutely necessary and ensure it's stored securely. Remove it from the container after copying (see next command).

---

### 🗑️ Remove Private Key from Container

Delete the root CA private key from the Docker container for security:

```sh
docker exec -it step-ca-management rm /home/step/secrets/root_ca_key
```

**Best Practice:** After copying the root key for backup or offline operations, remove it from the container to minimize security risks.

---

## 🔍 Additional Useful Commands

### Get CA Fingerprint

Retrieve the fingerprint of your root CA certificate:

```sh
docker exec step-ca-management step-cli certificate fingerprint /home/step/certs/root_ca.crt
```

### Request a Certificate

Request a new certificate from your Step CA:

```sh
step-cli ca certificate example.com example.crt example.key
```

### Check CA Health

Verify that your Step CA server is running and healthy:

```sh
step-cli ca health --ca-url=https://localhost:9000
```

---

## 📚 Additional Resources

- [Step CLI Documentation](https://smallstep.com/docs/step-cli)
- [Step CA Documentation](https://smallstep.com/docs/step-ca)
- [Certificate Management Best Practices](https://smallstep.com/docs/tutorials)
- [Step CLI Command Reference](https://smallstep.com/docs/step-cli/reference)

---

## 💡 Tips

- Use `step-cli path` to find your Step CLI configuration directory
- Add `--context <name>` to commands to work with specific CA contexts
- Use `step-cli help <command>` to get detailed help for any command
- Most commands support `--dry-run` flag to preview changes without applying them

# Red Hat Developer Hub with Ansible Automation Platform

This repository provides a complete integration between **Red Hat Developer Hub (RHDH)**, **Ansible Automation Platform (AAP)**, and **Red Hat SSO (Keycloak)** on OpenShift.

## Overview

Enable developers to seamlessly access both platforms with a single set of credentials and launch Ansible automation directly from Developer Hub.

### Key Features

- 🔐 **Single Sign-On** - Use Keycloak credentials across the entire platform
- 🎯 **OAuth Integration** - Authenticate to AAP through Developer Hub's Ansible plugin
- 🚀 **Self-Service Automation** - Launch Ansible workflows directly from Developer Hub
- 📊 **Catalog Sync** - View Ansible resources (job templates, inventories, projects) in the Developer Hub catalog
- 🔑 **Token-Based Security** - Secure API communication between Developer Hub and AAP

## Documentation

📖 **[Complete Integration Guide](docs/blogs/index.md)** - Step-by-step instructions covering:

- Infrastructure deployment
- Keycloak configuration
- AAP SSO setup
- OAuth application creation
- RHDH plugin configuration
- End-to-end authentication flow

## Repository Structure

```
├── catalog/             # Backstage catalog entities
├── clusters/demo/       # Kustomize overlays for deployment
├── docs/                # Documentation and guides
└── kustomize/           # Base configurations
```

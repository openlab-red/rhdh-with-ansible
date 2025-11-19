# Red Hat Developer Hub Configuration Files

This directory contains all configuration files for Red Hat Developer Hub (RHDH) that are managed via Kustomize's ConfigMapGenerator.

## File Structure

```
config/
├── app-config-app.yaml           # Application metadata (title, base URL)
├── app-config-backend.yaml       # Backend service configuration
├── app-config-organization.yaml  # Organization profile information
├── app-config-proxy.yaml         # Reverse proxy endpoints (AAP controller)
├── app-config-ansible.yaml       # Ansible Automation Platform integration
├── app-config-orchestrator.yaml  # SonataFlow orchestrator integration
├── app-config-integrations.yaml  # GitHub App integration details
├── app-config-catalog.yaml       # Catalog providers and sync rules
├── app-config-auth.yaml          # Authentication providers (Keycloak OIDC)
├── app-config-signin.yaml        # Sign-in page behavior
├── app-config-scaffolder.yaml    # Scaffolder defaults
├── app-config-kubernetes.yaml    # Kubernetes plugin setup
├── app-config-techdocs.yaml      # TechDocs builder configuration
├── app-config-permission.yaml    # RBAC/permission plugin configuration
├── app-config-enabled.yaml       # Feature flags and enabled modules
├── dynamic-plugins.yaml          # Dynamic plugin configuration
├── rbac-policies.csv             # RBAC role definitions and permissions
├── rbac-conditional-policies.yaml # Conditional RBAC policies
└── README.md                     # This file
```

## Files Description

### app-config-*.yaml
The primary Backstage configuration is split into focused files so overlays can override small sections without touching the rest. Each file maps to a top-level key in `app-config.yaml`:
- `app-config-app.yaml`: Application metadata such as title and `baseUrl`.
- `app-config-backend.yaml`: Backend base URL, CORS, and external access tokens.
- `app-config-organization.yaml`: Organization branding shown in Backstage.
- `app-config-proxy.yaml`: Reverse proxy targets (e.g., AAP controller API).
- `app-config-ansible.yaml`: Ansible Automation Platform, creator service, and automation hub settings.
- `app-config-orchestrator.yaml`: SonataFlow Data Index endpoint for the orchestrator plugin.
- `app-config-integrations.yaml`: GitHub App host, credentials, and private key reference.
- `app-config-catalog.yaml`: Catalog rules, providers (GitHub, RHAAP, Keycloak), and sync schedules.
- `app-config-auth.yaml`: Authentication configuration for Keycloak OIDC and backend secrets.
- `app-config-signin.yaml`: Sign-in page defaults and redirect flow toggle.
- `app-config-scaffolder.yaml`: Default author metadata and commit message used by templates.
- `app-config-kubernetes.yaml`: Kubernetes plugin clusters and service account configuration.
- `app-config-techdocs.yaml`: TechDocs builder, generator, and publisher mode.
- `app-config-permission.yaml`: RBAC/permission plugin settings and admin assignments.
- `app-config-enabled.yaml`: Feature/module toggles (e.g., Keycloak backend module).

### dynamic-plugins.yaml
Defines which plugins are enabled and their configuration:
- Ansible plugins (UI and scaffolder backend)
- GitHub catalog discovery module
- Bulk import plugins
- Keycloak backend module
- RBAC plugin
- Orchestrator plugins (SonataFlow integration)
- Notification and signals plugins

### rbac-policies.csv
CSV file containing role-based access control policies:
- Permission definitions
- Role assignments
- Policy rules for catalog entities

### rbac-conditional-policies.yaml
YAML file with conditional RBAC policies:
- Advanced permission rules with conditions
- Dynamic policy evaluation based on context

## How It Works

These files are used by Kustomize's `configMapGenerator` defined in the parent `kustomization.yaml`. When you run `kubectl apply -k .` or `oc apply -k .`, Kustomize will:

1. Generate ConfigMaps from these files
2. Apply the ConfigMaps to the namespace
3. Mount them into the RHDH pods at the appropriate paths

## Modifying Configuration

To modify any configuration:

1. Edit the appropriate file in this directory
2. Apply the changes using:
   ```bash
   oc apply -k /path/to/kustomize/instances/rhdh/
   ```
3. The RHDH pods will automatically reload most configuration changes

## Environment Variables

Many values in these configurations use environment variables (e.g., `${BASE_URL}`, `${AAP_TOKEN}`). These are resolved at runtime from secrets and environment configuration in the RHDH deployment.

## Related Files

- `../kustomization.yaml` - Kustomize configuration that references these files
- `../secret-*.yaml` - Secret definitions for sensitive data
- `../backstage.yaml` - RHDH deployment configuration

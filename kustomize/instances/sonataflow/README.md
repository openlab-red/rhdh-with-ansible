# SonataFlow Data Index Instance

This directory contains the configuration for deploying the SonataFlow Data Index service, which is required by the Red Hat Developer Hub Orchestrator plugin.

## Prerequisites

1. SonataFlow Operator must be installed (see `../../operators/sonataflow-operator/`)
2. The operator must be running in the `sonataflow-operator-system` namespace

## Deployment

### Step 1: Install the Operator (if not already done)

```bash
cd ../../operators/sonataflow-operator
# Follow instructions in README.md to install the operator
```

### Step 2: Deploy Data Index Instance

```bash
# Apply the Data Index instance
kubectl apply -k .

# Wait for the Data Index to be ready
kubectl wait --for=condition=ready sonataflow/data-index -n sonataflow-operator-system --timeout=300s
```

### Step 3: Verify Deployment

```bash
# Check SonataFlow instance status
kubectl get sonataflow -n sonataflow-operator-system

# Check pods
kubectl get pods -n sonataflow-operator-system | grep data-index

# Check service
kubectl get svc -n sonataflow-operator-system | grep data-index

# Check route
kubectl get route -n sonataflow-operator-system | grep data-index
```

### Step 4: Get Data Index URL

```bash
# Get the route URL
DATA_INDEX_URL=$(kubectl get route sonataflow-data-index -n sonataflow-operator-system -o jsonpath='{.spec.host}')
echo "Data Index URL: https://${DATA_INDEX_URL}"

# Or get the full URL with protocol
DATA_INDEX_FULL_URL="https://$(kubectl get route sonataflow-data-index -n sonataflow-operator-system -o jsonpath='{.spec.host}')"
echo "Full URL: ${DATA_INDEX_FULL_URL}"
```

### Step 5: Configure RHDH Orchestrator Plugin

Update the RHDH Backstage deployment to include the Data Index URL:

```bash
# Edit the backstage.yaml in ../rhdh/
# Add or update the ORCHESTRATOR_DATA_INDEX_URL environment variable

# Example: Update backstage.yaml
kubectl patch backstage developer-hub -n rhdh --type='json' -p='[
  {
    "op": "add",
    "path": "/spec/application/extraEnvs/envs/-",
    "value": {
      "name": "ORCHESTRATOR_DATA_INDEX_URL",
      "value": "https://sonataflow-data-index-sonataflow-operator-system.<your-cluster-domain>"
    }
  }
]'
```

Or update the `backstage.yaml` file directly:

```yaml
extraEnvs:
  envs:
    - name: ORCHESTRATOR_DATA_INDEX_URL
      value: "https://sonataflow-data-index-sonataflow-operator-system.<your-cluster-domain>"
```

## Troubleshooting

### Check Data Index Logs

```bash
kubectl logs -n sonataflow-operator-system -l app=sonataflow-data-index --tail=100
```

### Check Operator Logs

```bash
kubectl logs -n sonataflow-operator-system -l control-plane=controller-manager --tail=100
```

### Verify Data Index Health

```bash
# Get the route URL
DATA_INDEX_URL=$(kubectl get route sonataflow-data-index -n sonataflow-operator-system -o jsonpath='{.spec.host}')

# Check health endpoint
curl -k https://${DATA_INDEX_URL}/q/health
```

## Configuration

The Data Index is configured with:
- **Profile**: `dev` (change to `prod` for production)
- **Memory**: 512Mi request, 1Gi limit
- **CPU**: 250m request, 500m limit

To modify these settings, edit `sonataflow-data-index.yaml`.

## Documentation

- [SonataFlow Operator Documentation](https://sonataflow.org/serverlessworkflow/latest/cloud/operator/)
- [RHDH Orchestrator Plugin](https://github.com/redhat-developer/rhdh-plugins/tree/main/workspaces/orchestrator)


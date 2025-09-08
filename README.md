# Catalog Orders

GitOps repository for Crossplane Resource (XR) instances created via Backstage (app-portal). Flux monitors this repository and automatically syncs XRs to the Kubernetes cluster.

## Purpose

This repository stores XR instances (resource orders) that are:
- Created through Backstage scaffolder templates in app-portal
- Organized by cluster context and namespace/purpose
- Automatically deployed by Flux to the corresponding Kubernetes cluster

## Path Structure

```
catalog-orders/
├── {cluster-context}/        # e.g., rancher-desktop, openportal, kind-openportal
│   ├── system/              # Platform-level resources
│   │   └── ManagedNamespace/   # Namespace provisioning XRs
│   │       └── {name}.yaml
│   ├── demo/                # Demo and example resources
│   │   ├── CloudflareDNSRecord/
│   │   ├── WhoAmIApp/
│   │   └── WhoAmIService/
│   └── test/                # Test resources
│       └── {XR-type}/
│           └── {name}.yaml
```

**Note:** The cluster context matches your kubectl context name (e.g., `rancher-desktop`, `openportal`)

## Adding XRs

XRs are **not manually added** to this repository. Instead:

1. **Use Backstage (app-portal)**:
   - Navigate to http://localhost:3000/create
   - Select a template (e.g., "DNS Record", "WhoAmI App")
   - Fill in the required parameters
   - Choose your target cluster
   - Submit to create a PR automatically

2. **Backstage creates a PR** with:
   - Properly formatted XR YAML
   - Correct path based on cluster and resource type
   - Backstage annotations for catalog integration

3. **Merge the PR** to deploy:
   - Flux detects the change
   - XR is applied to the cluster
   - Resources are provisioned according to the Composition

## Example XR

```yaml
apiVersion: openportal.dev/v1alpha1
kind: WhoAmIApp
metadata:
  name: my-app
  annotations:
    terasky.backstage.io/source-info: '...'  # Added by Backstage
spec:
  id: my-app
  host: my-app.example.com
```

## Flux Integration

Flux is configured to watch paths based on the cluster context:
- `/rancher-desktop/**` → Rancher Desktop cluster
- `/openportal/**` → OpenPortal production cluster
- `/kind-openportal/**` → Kind test cluster

Changes merged to `main` are automatically synced within ~1 minute.
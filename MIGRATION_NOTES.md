# ManagedNamespace v2.1.0 Migration

## Overview

This repository has been updated to support ManagedNamespace v2.1.0, which changes from cluster-scoped to namespaced resources.

## Directory Structure

```
catalog-orders/
├── <cluster-name>/
│   ├── system/                    # Infrastructure XRs (namespaced in 'system')
│   │   └── ManagedNamespace/       # Namespace management XRs
│   │       └── *.yaml
│   └── <namespace>/                # Application XRs (in their respective namespaces)
│       └── <Kind>/
│           └── *.yaml
```

## Changes in v2.1.0

### Breaking Changes
- **Scope**: Changed from cluster-scoped to namespaced
- **Location**: ManagedNamespace XRs now reside in 'system' namespace
- **Directory**: Moved from `cluster-scoped/` to `system/`

### Migration
All existing ManagedNamespace XRs have been migrated to:
- Directory: `system/ManagedNamespace/`
- Namespace: `system` (added to metadata)

## Examples

### Creating a ManagedNamespace
```yaml
apiVersion: openportal.dev/v1alpha1
kind: ManagedNamespace
metadata:
  name: my-namespace
  namespace: system  # Required - always in system namespace
spec:
  name: my-namespace
```

### Directory Placement
- Infrastructure XRs: `<cluster>/system/<Kind>/<name>.yaml`
- Application XRs: `<cluster>/<namespace>/<Kind>/<name>.yaml`

## Related Changes
- portal-workspace PR #86: Added system namespace creation
- catalog PR #33: Updated template-namespace to v2.1.0
- template-namespace release v2.1.0: Changed to namespaced scope
# Kubernaut Test Playbooks

Ansible playbooks used by Kubernaut to validate the Ansible execution engine (BR-WE-015) and run GitOps remediation workflows.

## Playbooks

### E2E test playbooks

| Playbook | Purpose |
|----------|---------|
| `playbooks/test-success.yml` | Runs successfully with a brief simulated workload |
| `playbooks/test-failure.yml` | Fails intentionally for failure-path testing |
| `playbooks/test-dep-configmap.yml` | Validates ConfigMap dependency injection via AWX extra\_vars |
| `playbooks/test-dep-secret.yml` | Validates Secret dependency injection via AWX credential env vars |

### GitOps remediation playbooks

| Playbook | Purpose |
|----------|---------|
| `playbooks/gitops-update-memory-limits.yml` | Commits updated memory limits to a GitOps repo via ArgoCD |
| `playbooks/gitops-migrate-postgres-emptydir-to-pvc.yml` | Migrates PostgreSQL emptyDir volumes to PVC with backup/restore via GitOps |

## Usage

These playbooks are referenced by Kubernaut's AWX integration via:
- **AWX Project**: Git SCM pointing to this repository
- **AWX Job Template**: Associates a playbook path with an inventory and credentials
- **Kubernaut EngineConfig**: `jobTemplateName` field in workflow schema (resolves the AWX job template by name)

## Parameters

### Standard target resource parameters (DD-WORKFLOW-003)

| Variable | Type | Description |
|----------|------|-------------|
| `TARGET_RESOURCE_NAMESPACE` | string | Kubernetes namespace of the target resource |
| `TARGET_RESOURCE_NAME` | string | Name of the target resource |
| `TARGET_RESOURCE_KIND` | string | Kind of the target resource (e.g., Deployment) |

### AWX credentials required by GitOps playbooks

The GitOps playbooks use `kubernetes.core` modules that require cluster access. The WE controller creates an ephemeral AWX credential from its in-cluster identity that injects a kubeconfig file:

| Env var | Description |
|---------|-------------|
| `K8S_AUTH_KUBECONFIG` | Path to a generated kubeconfig file containing the API server URL, CA cert, and bearer token |

A **Gitea credential** is also attached (via `dependencies.secrets`), injecting:

| Env var | Description |
|---------|-------------|
| `KUBERNAUT_SECRET_GITEA_REPO_CREDS_USERNAME` | Git username |
| `KUBERNAUT_SECRET_GITEA_REPO_CREDS_PASSWORD` | Git password |

## License

Apache License 2.0 — see [kubernaut](https://github.com/jordigilh/kubernaut) for details.

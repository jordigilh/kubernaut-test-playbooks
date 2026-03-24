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
| `playbooks/gitops-migrate-emptydir-to-pvc.yml` | Migrates emptyDir volumes to PVC with backup/restore via GitOps |

## Usage

These playbooks are referenced by Kubernaut's AWX integration via:
- **AWX Project**: Git SCM pointing to this repository
- **AWX Job Template**: References a specific playbook path
- **Kubernaut EngineConfig**: `playbookPath` field in workflow schema

## Parameters

### Standard target resource parameters (DD-WORKFLOW-003)

| Variable | Type | Description |
|----------|------|-------------|
| `TARGET_RESOURCE_NAMESPACE` | string | Kubernetes namespace of the target resource |
| `TARGET_RESOURCE_NAME` | string | Name of the target resource |
| `TARGET_RESOURCE_KIND` | string | Kind of the target resource (e.g., Deployment) |

### AWX credentials required by GitOps playbooks

The GitOps playbooks use `kubernetes.core` modules that require cluster access. The WE controller attaches an **AAP "OpenShift or Kubernetes API Bearer Token" credential** to the job template, which injects:

| Env var | Description |
|---------|-------------|
| `K8S_AUTH_HOST` | Kubernetes API server URL |
| `K8S_AUTH_API_KEY` | ServiceAccount bearer token |
| `K8S_AUTH_SSL_CA_CERT` | Cluster CA certificate |

A **Gitea credential** is also attached (via `dependencies.secrets`), injecting:

| Env var | Description |
|---------|-------------|
| `KUBERNAUT_SECRET_GITEA_REPO_CREDS_USERNAME` | Git username |
| `KUBERNAUT_SECRET_GITEA_REPO_CREDS_PASSWORD` | Git password |

## License

Apache License 2.0 — see [kubernaut](https://github.com/jordigilh/kubernaut) for details.

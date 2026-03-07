# Kubernaut Test Playbooks

Ansible playbooks used by Kubernaut E2E tests to validate the Ansible execution engine (BR-WE-015).

## Playbooks

| Playbook | Purpose |
|----------|---------|
| `playbooks/test-success.yml` | Runs successfully with a brief simulated workload |
| `playbooks/test-failure.yml` | Fails intentionally for failure-path testing |

## Usage

These playbooks are referenced by Kubernaut's AWX integration via:
- **AWX Project**: Git SCM pointing to this repository
- **AWX Job Template**: References a specific playbook path
- **Kubernaut EngineConfig**: `playbookPath` field in workflow schema

## Parameters

Both playbooks accept `extra_vars` passed from Kubernaut workflow parameters:

| Variable | Type | Description |
|----------|------|-------------|
| `target_namespace` | string | Kubernetes namespace of the target resource |
| `target_name` | string | Name of the target resource |
| `target_kind` | string | Kind of the target resource (e.g., Deployment) |

## License

Apache License 2.0 — see [kubernaut](https://github.com/jordigilh/kubernaut) for details.

# env-sba GitOps

Kubernetes manifests for env-sba, managed by ArgoCD.

## Apply manually (for debugging)

```bash
kubectl apply -f manifests/
```

## Source of truth

ArgoCD watches the `main` branch of this repo and syncs the `manifests/` directory.

## Files

- `manifests/namespace.yaml` — creates the `env-sba` namespace
- `manifests/serviceaccount.yaml` — ServiceAccount + Role + RoleBinding for Spring Cloud Kubernetes
- `manifests/configmap.yaml` — application config injected by Spring Cloud Kubernetes
- `manifests/secret.yaml` — basic auth credentials (replace with Sealed Secrets / ESO before prod)
- `manifests/deployment.yaml` — the Spring Boot workload
- `manifests/service.yaml` — ClusterIP Service with SBA client metadata labels
- `manifests/ingress.yaml` — exposes the service via nginx Ingress at `env-sba.local`

## Bumping the image version

Edit `manifests/deployment.yaml`, change the `image:` tag, commit, push.
ArgoCD will sync within ~3 minutes (or instantly with a webhook).

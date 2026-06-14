# Argo CD Example

Example repository for deploying the **product-catalog** microservice with [Argo CD](https://argo-cd.readthedocs.io/).

Each example lives in its own folder under `argocd-apps/`:

| Example | Path | Description |
|---|---|---|
| Plain YAML | `argocd-apps/plain-yaml/` | Single Application deploying plain Kubernetes manifests |
| Helm | `argocd-apps/helm/` | Single Application deploying a Helm chart |
| App of Apps | `argocd-apps/app-of-apps/` | Root Application that manages multiple child Applications |

## Prerequisites

- `kubectl` configured for your target cluster

## 1. Create namespace

```bash
kubectl create namespace argocd
```

## 2. Install Argo CD

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Wait until pods are ready:

```bash
kubectl get pods -n argocd -w
```

## 3. Initial admin password

The default username is `admin`. Decode the initial password from the secret:

**Linux / macOS / Git Bash**

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

**Windows PowerShell**

```powershell
$pw = kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"
[System.Text.Encoding]::UTF8.GetString([Convert]::FromBase64String($pw))
```

## 4. Port-forward the API server

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open **https://localhost:8080** in a browser (accept the self-signed certificate warning). Log in as `admin` with the password from step 3.

## 5. Deploy an example

### Plain YAML

Deploys the product-catalog microservice from plain Kubernetes manifests.

```bash
kubectl apply -f argocd-apps/plain-yaml/application.yaml
```

### Helm

Deploys the product-catalog microservice from a Helm chart with environment-specific values.

```bash
kubectl apply -f argocd-apps/helm/application.yaml
```

### App of Apps

A single root Application that automatically discovers and manages child Applications from the `argocd-apps/app-of-apps/apps/` directory. Adding or removing an Application YAML in that directory is all it takes to onboard or remove an app.

```bash
kubectl apply -f argocd-apps/app-of-apps/root-application.yaml
```

## References

- [Argo CD getting started](https://argo-cd.readthedocs.io/en/stable/getting_started/)

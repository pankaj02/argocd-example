# Argo CD Example

Example repository for deploying the **product-catalog** microservice with [Argo CD](https://argo-cd.readthedocs.io/).

Two deployment styles are included:

| Application manifest | Style | Source path |
|---|---|---|
| `argocd-apps/application.yaml` | Plain YAML | `product-catalog/deployments` |
| `argocd-apps/application-helm.yaml` | Helm chart | `product-catalog/helm/product-catalog` |

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

## 5. Deploy the applications

```bash
# Plain-YAML deployment
kubectl apply -f argocd-apps/application.yaml

# Helm-based deployment
kubectl apply -f argocd-apps/application-helm.yaml
```

## References

- [Argo CD getting started](https://argo-cd.readthedocs.io/en/stable/getting_started/)

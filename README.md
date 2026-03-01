# argocd-lab-gitops

#### 1. Clone

```bash
git clone https://github.com/BacktotheBone/argocd-lab.git
cd argocd-lab-gitops
```

#### 2. Deploy ArgoCD

```bash
kubectl apply --server-side -k argocd/
```

This command:
- Creates the `argocd` namespace
- Installs ArgoCD using the official manifests (v3.3.1)

Wait for ArgoCD to be ready before proceeding:

```bash
kubectl wait --for=condition=available --timeout=300s deployment/argocd-server -n argocd
```

#### 3. Deploy the App-of-Apps bootstrap

```bash
kubectl apply -f app-of-apps/gitops-app.yaml -n argocd
```

#### 4. Access ArgoCD

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open: `https://localhost:8080`

**Credentials:**
- Username: `admin`
- Password:
  ```bash
  kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
  ```
# app-of-apps

App of Apps enables you to manage all application with ArgoCD, more specifically ArgoCD will manage the application definitions, the projects and the application deployments.  

The instructions below are written for MacOS.  

- Install ArgoCD to your local cluster
```
helm repo add argo https://argoproj.github.io/argo-helm
helm upgrade --install my-argo-cd argo/argo-cd --version 6.6.0 --create-namespace
```

- Launch ArgoCD in your browser  
```
kubectl -n default get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d | pbcopy && kubectl port-forward service/my-argo-cd-argocd-server -n default 8080:443 & open http://localhost:8080
```
Login with admin and the password in your clipbpoard.

- Install the Argo CLI
```
brew install argocd
```



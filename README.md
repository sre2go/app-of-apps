# app-of-apps

App of Apps enables you to manage all application with ArgoCD, more specifically ArgoCD will manage the application definitions, the projects and the application deployments.  

The instructions below are written for MacOS.  

- Install ArgoCD to your local cluster and let it monitor itself
```
# Do an initial install of ArgoCD
helm repo add argo https://argoproj.github.io/argo-helm
helm upgrade --install argo argo/argo-cd --version 6.6.0 --namespace argocd --create-namespace --values argocd/values.yaml
# Create the App of Apps project which will monitor the git repo defined within.
kubectl apply -f aoa.yaml
```

- Launch ArgoCD in your browser  
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d | pbcopy && kubectl port-forward service/argo-argocd-server -n argocd 8080:443 & open http://localhost:8080
```
Login with admin and the password in your clipbpoard.

- Install the Argo CLI and login
```
brew install argocd
argocd login localhost:8080
```

- Add you App of Apps GIT repo  
Create a secret with the SSH private key you want ArgoCD to use your repo:  
```
apiVersion: v1
kind: Secret
metadata:
  name: app-of-apps-repo
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repo-creds
stringData:
  type: git
  url: git@github.com:sre2go/app-of-apps.git
  sshPrivateKey: |
    -----BEGIN OPENSSH PRIVATE KEY-----
    blahblahblah
    -----END OPENSSH PRIVATE KEY-----
```



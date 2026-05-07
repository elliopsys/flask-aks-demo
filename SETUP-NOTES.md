# Flask AKS Demo Setup Notes

## Azure resources used
Resource group: flask-aks-rg-sea
AKS cluster: flask-aks-cluster
ACR: flaskelli1
Namespace: flask-demo
App: flask-app
Image tag: v1

## Manual deployment flow
1. Create resource group
2. Create ACR
3. Create AKS with allowed VM size
4. Attach ACR to AKS
5. Build Docker image on Mac using linux/amd64
6. Push image to ACR
7. Deploy Kubernetes manifests
8. Expose app using LoadBalancer

## Important fix used
For Apple Silicon or platform mismatch:

docker buildx build \
  --platform linux/amd64 \
  -t <acr-login-server>/flask-app:v1 \
  --push .

## Completed
- Manual Flask deployment to AKS
- Azure DevOps CI/CD pipeline

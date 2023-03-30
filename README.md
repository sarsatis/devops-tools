# devops-tools

# Jenkins
##### To bring up jenkins use below command (Note Remember to create dockercred)

> kubectl create secret docker-registry dockercred --docker-server=https://index.docker.io/v1/ --docker-username=sarthaksatish --docker-password=***** --docker-email=sarthak8055@gmail.com

> helm install --create-namespace jenkins jenkins/

# Azure VM
> Azure VM can be spin up using template and parameter json and use install-script to install kubernetes and jenkins on that server

## Create Static Public IP In Azure
```t
# Get the resource group name of the AKS cluster 
az aks show --resource-group sarthak-cluste_group --name devops-aks-tools --query nodeResourceGroup -o tsv

# TEMPLATE - Create a public IP address with the static allocation
az network public-ip create --resource-group <REPLACE-OUTPUT-RG-FROM-PREVIOUS-COMMAND> --name myAKSPublicIPForIngress --sku Standard --allocation-method static --query publicIp.ipAddress -o tsv

# REPLACE - Create Public IP: Replace Resource Group value
az network public-ip create --resource-group MC_sarthak-cluste_group_devops-aks-tools_eastus --name myAKSPublicIPForIngress --sku Standard --allocation-method static --query publicIp.ipAddress -o tsv
```
- Make a note of Static IP which we will use in next step when installing Ingress Controller
```t
# Make a note of Public IP created for Ingress
20.169.187.220
```

## Install Ingress Controller
```t
# Install Helm3 (if not installed)
brew install helm

# Create a namespace for your ingress resources
kubectl create namespace ingress-nginx

# Add the official stable repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

#  Customizing the Chart Before Installing. 
helm show values ingress-nginx/ingress-nginx

# Use Helm to deploy an NGINX ingress controller
helm install ingress-nginx /ingress-nginx \
    --namespace ingress-nginx \
    --set controller.replicaCount=1 \
    --set controller.nodeSelector."kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."kubernetes\.io/os"=linux \
    --set controller.service.externalTrafficPolicy=Local \
    --set controller.service.loadBalancerIP="REPLACE_STATIC_IP" 

# Replace Static IP captured in Step-02 (without beta for NodeSelectors)
helm install ingress-nginx /ingress-nginx \
    --namespace ingress-nginx \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."kubernetes\.io/os"=linux \
    --set controller.service.externalTrafficPolicy=Local \
    --set controller.service.loadBalancerIP="20.169.187.220"    
```

# Istio on Azure VM

> curl -Ls https://istio.io/downloadIstio | ISTIO_VERSION=1.9.0 sh -
> cd istio-1.9.0/
> export PATH=$PWD/bin:$PATH
> istioctl install --set profile=demo -y && kubectl apply -f samples/addons


All the devops tools

1. Deployed nginx - k apply -f nginx.yaml
2. Add this in ingress.yaml kubernetes.io/ingress.class: "nginx"
3. kustomize build argo-cd/overlays/production | k apply -f -k
4. export PASS=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)
5. export INGRESS_HOST=127.0.0.1
6. argocd login --insecure --username admin --password $PASS --grpc-web argo-cd.$INGRESS_HOST.nip.io
7. open http://argo-cd.$INGRESS_HOST.nip.io
8. k apply -f project.yaml
9. k apply -f apps.yaml
10. add this in argo-workflows.yaml extraArgs: - --auth-mode=server
11. brew install argo

12. argo --namespace workflows submit \
    workflows/cd-mock.yaml

13. argo --namespace workflows list

14. argo --namespace workflows \
    get @latest

15. argo --namespace workflows \
    logs @latest \
    --follow

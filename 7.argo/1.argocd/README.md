# ArgoCD

## Step 1 :- Installation

```t
# Install Helm3 (if not installed)
brew install helm

# Create a namespace for your ingress resources
kubectl create namespace argocd

# Using kubectl
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
# Add the official stable repository

# Using Helm 
helm repo add argo https://argoproj.github.io/argo-helm 
helm repo update

#  Customizing the Chart Before Installing. 
helm show values argo/argo-cd

helm install argocd argo/argo-cd -n argocd

Sample Logs
NAME: argocd
LAST DEPLOYED: Fri Apr 14 01:44:31 2023
NAMESPACE: argocd
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
In order to access the server UI you have the following options:

1. kubectl port-forward service/argocd-server -n argocd 8081:80

    and then open the browser on http://localhost:8080 and accept the certificate

2. enable ingress in the values file `server.ingress.enabled` and either
      - Add the annotation for ssl passthrough: https://argo-cd.readthedocs.io/en/stable/operator-manual/ingress/#option-1-ssl-passthrough
      - Set the `configs.params."server.insecure"` in the values file and terminate SSL at your ingress: https://argo-cd.readthedocs.io/en/stable/operator-manual/ingress/#option-2-multiple-ingress-objects-and-hosts


After reaching the UI the first time you can login with username: admin and the random password generated during the installation. You can find the password by running:

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

(You should delete the initial secret afterwards as suggested by the Getting Started Guide: https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli)

````

## Step 2 :- Create an ingress to access argocd 

```t
k apply -f 7.argo/1.argocd/argocd-ingress.yaml 
```

## ArgoCD Commands

```t
argocd login --insecure --username admin --password 0ZYcYh5DgyV0-ly1 --grpc-web argocd.simplifydevopstools.com --skip-test-tls 

argocd account update-password --current-password 0ZYcYh5DgyV0-ly1 --new-password (Give Your Pass)

argocd cluster list

Add context to kube config and add clusters to argocd using below commands
argocd cluster add devops-tools-cluster

Once the clusters are added to argo cd they are stored as secret in argocd namespace

# This is for PR Generator
k create secret generic github-token --from-literal=token=PATTOKEN

kubectl config delete-context gke_fsi-retailbanking-dev_us-central1_infratest-gke

 kubectl config delete-cluster gke_fsi-retailbanking-dev_us-central1_infratest-gke
```
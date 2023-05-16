kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml


Kubectl argo plugin
brew install argoproj/tap/kubectl-argo-rollouts

kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-rollouts/master/docs/getting-started/basic/rollout.yaml

kubectl argo rollouts get rollout rollouts-demo --watch
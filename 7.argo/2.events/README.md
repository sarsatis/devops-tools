#################################################

# Argo Events

# Event-Based Dependency Manager for Kubernetes

# https://youtu.be/sUPkGChvD54

#################################################

#########

# Setup

#########

# It could be any Kubernetes cluster

kubectl create namespace argo-events

kubectl apply --filename https://raw.githubusercontent.com/argoproj/argo-events/stable/manifests/install.yaml

kubectl --namespace argo-events apply --filename https://raw.githubusercontent.com/argoproj/argo-events/stable/examples/eventbus/native.yaml

git clone https://github.com/vfarcic/argo-events-demo.git

cd argo-events-demo

##########################

# Creating event sources

##########################

cat event-source.yaml

kubectl --namespace argo-events apply --filename event-source.yaml

kubectl --namespace argo-events get eventsources

kubectl --namespace argo-events get services

kubectl --namespace argo-events get pods

kubectl --namespace argo-events apply -f argoevents-ingress.yaml

curl -X POST \
 -H "Content-Type: application/json" \
 -d '{"message":"My first webhook"}' \
 https://20.231.85.16/devops-tools

wget -O- --post-data='{"message":"My first webhook"}' --header='Content-Type:application/json' 'http://webhook-eventsource-svc:80/devops-tools'

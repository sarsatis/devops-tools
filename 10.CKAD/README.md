# 1. Everything Else

## Helpers

```yaml
--dry-run=client -o yaml

--grace-period=0 -o yaml

use shift+$ to go to end in yaml

use / to find correct name in yaml

use wc -l to know the count

use of -A3 and -B3 to know after and before lines while using describe

Use '#' While using commands

k config set-context --current --namespace Do not use this

while using describe navigate to respective section fastly

Remember to put space after --

set number to see line numbers in vi editor

while using -c if command contains spaces please use quotes

if gibberish comes it means already its running please delete it and then recreate dont try to fix something else

if your stuck in some issue please move on dont waste time on it once you complete then come to marked questions

Check if you have fixed the issue after fixed
```

# 2. Core Concepts

## Pod

```yaml
k run name --image imagename
k get po -o wide
k describe po name
k delete po name
k edit po name && k replace --force -f filepath
```

## ReplicaSet

```yaml
k get rs
k describe rs name
RS Ensures that desired count and expected count is always met
RS apiVersion is apps/v1

RS selectors should match the labels
k delete rs rsname
k edit rs name

after editing something in rs remember to delete pods so that it recreates from newly edited yaml (This should be only done to rs deploy will automatically deploy to latest edited)

k scale rs name --replicas 2
```

## Deployment

```yaml
k get deploy
k describe deploy name

Deploy selectors should match the labels

k create deploy name --image nginx --replicas 3

Deploy --> RS --> PO
k set image deploy deployname containername=newimage

2 strategies
recreate and rolling update

Rolling update is the default strategy
type: Rolling Update
rollingUpdate:
   maxSurge: 25%
   maxUnavailable: 25%
```

## Namespaces

```yaml
k get ns
k get po -n namespace
k run redis -n namespace --image redis
k create ns name
In same namespace use svc name itself. In different namespace use fully qualified name db.dev.svc.cluster.local
```

## Imperative Commands

```yaml
k run redis --image redis -l tier=db (Labels only applicable while creating po not during deployment)

k get po --show-labels

k expose po/deploy name --name svcname --port 6379 --target-port 6379 --type ClusterIP/NodePort/LoadBalancer

k run name --image nginx --port 8080 --expose (We can specify port during run command itself) (We can only use expose if we have same name to svc and pod because using expose will use same name while creating svc)

k run httpd --image httpd --port 80 --expose
```

# 3. Configurations

## Args:-

```yaml
Use /bin/sh -c while running any command only while using busybox image

CMD in docker == Args in k8's

Sec 1

ENTRYPOINY pyhon app.py
CMD --color red
command in k8's
command --color green

Sec 2

ENTRYPOINY pyhon app.py
CMD --color red

command in k8's

command pyhon app.py
args --color green

k run nginx --image nginx -- --color green (Passing args)
k run nginx --image nginx --command --color green (Passing args)
```

## ConfigMap :-

```yaml

k create cm cmname --from-literal APP=RED

k create cm cmname --form-file=filename=filepath

1. Normal env
env:
- name:
  value:
---

2. ConfigMap env which takes all env variables
envFrom:
- configMapRef:
    name:
---
3. ConfigMap env which takes specific key values
env:
- name: DB_USER_NAME
  valueFrom:
    configMapKeyRef:
      key: key to fetch from config map
      name: name of configmap
```

## Secrets

```yaml
k create secret generic/docker-registry/tls name --from-literal=PASS=123
k create secret regcred name --help

1. Normal env
env:
- name:
  value:
2. Secret which takes all env variables
envFrom:
- secretRef:
    name:
3. SecretKeyRef env which takes specific key values
env:
- name: DB_USER_NAME
  valueFrom:
    secretKeyRef:
      key: key to fetch from secret
      name: name of secret
```

## Multiple possibilities

```yaml
Sec 1
env:
- name:
  value:
- name: DB_USER_NAME
  valueFrom:
    secretKeyRef:
      key: key to fetch from secret
      name: name of secret
- name: DB_USER_NAME
  valueFrom:
    configMapKeyRef:
      key: key to fetch from config map
      name: name of configmap
Sec 2
envFrom:
- secretRef:
    name:
- configMapRef:
    name:
```

## Security Contexts

```yaml
k exec poname -- whoami
Editing sec context of a running pod is not possible so first get yaml
k get po ubuntu -o yaml > po.yaml

spec:
  securityContext:
    runAsUser: 1001
    runAsGroup: 1001
    fsGroup: 1001
    capabilities:
      add: ["SYS_TIME", "NET_ADMIN"]
  containers:
  -
k replace --force -f po.yaml
Security Contexts can be defined both outside the containers section and inside the container section
```

## Resource Limits

```yaml
containers:
- name: sa
  resources:
    requests:
       memory: 20Mi
       cpu: 2
    limits:
       memory: 30Mi
       cpu: 3
```

## Service Account

```yaml
k get sa
k create sa saname
k create token saname
service account can only be seen in pod so describe pod to identify the sa
service acconts will always be mounted so check mounts

This spec is is of containers one REMEMBER

spec:
  serviceAccountName: saname
```

## Taints and Tolerations

```yaml
k taint node node01 key=value:NoSchedule
k taint node node01 key=value:NoSchedule- To delete a taint
Taints are applied to nodes. Tolerations are applied to pods
containers:
  - image: nginx
tolerations:
  - key: "example-key"
    operator: "Exists/Equal"
    value: "example-value"
    effect: "NoSchedule"
```

## Node Affinity

```yaml
k get po --show-labels
k label node node01 color=blue
k get po -o wide to check if the node is placed correctly
In node affinity we use labels

spec:
  containers:
  - image: nginx
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:   (There are 3 varities in it remember to give what they ask)
        nodeSelectorTerms:
        - matchExpressions:
          - key: topology.kubernetes.io/zone
            operator: In/Exists
            values:
            - antarctica-east1
            - antarctica-west1
```

# Multipod Container

## Multicontainer

```yaml
k logs podname -c containername

Probes
redinessProbe:
  httpGet:
    path: /
    port: 8080
  initialDelaySeconds: 10
  periodSecond: 1
livenessProbe:
  httpGet:
    path: /
    port: 8080
  initialDelaySeconds: 10
  periodSecond: 1
redinessProbe:
  exec:
    command:
    - cat
  initialDelaySeconds: 10
  periodSecond: 1
livenessProbe:
  exec:
    command:
    - cat
  initialDelaySeconds: 10
  periodSecond: 1
```

## Logging

```yaml
k logs podname -c containername
```

## Monitoring

```yaml
k top node
k top po
```

## Init containers

```yaml
Init containers will not always show up in 1/1 you will have to describe and see

Once the process is completed by init container it will go into terminated state
```

# Pod Design

## Labels and selectors

```yaml
k get po --show-labels
k get po -l env=dev
k get all -l env=prod (When using all dont use wc -l)
k get po -l env=prod,bu=finance,tier=frontend (Using multiple labels in a command use ,)
k label po podname key- (To remove label from a pod/deploy etc)
```

## Rolling Updates and RollBack

```yaml
k rollout undo deploy deployname
k rollout status deploy deployname
k rollout history deploy deployname --to-revision --record
```

## Jobs and CJ

```yaml
k create job jobname --image imagename
backofflimit to increase the number of times the job can run

check completions to see how many attempts it took

completions :- to only stop the once the specified completions is completed

remember to take dryrun of job and apply direcly editing will not work sometimes

parallelism

k create cj cjname --image imagename --schedule "30 21 * * *"
```

# Services and Networking

## Services

```yaml
k get svc
default svc is ClusterIp
k expose deploy deployname --name svcname --port 8080 --target-port 8080 --type NodePort
you can only modify node port by editing it
check if pod ips are matching with endpoints in svc
if node port use curl nodeid:nodeport
if clusterip use curl svcname:port or podip:port
```

## Network Policies

```yaml
Did lot of practice

remember to see labels

always run exec commands before you start creating netpol so that you know what to create when you do and also it will be easier later to test after created because you will have command
```

## Ingress

```yaml
k get ing

deploy ing where applications are deployed

k describe ing ingname

k edit ing ingname

annotations:
   nginx.ingress.kubernetes.io/rewrite-target: /
   nginx.ingress.kubernetes.io/ssl-redirect: "false"

k create ing ingname --rule=/pay:svcname:8282 (Ingress should always be in the same namespace wgere ods are deployed)
```

# State Persistence

```yaml
if you mistakenly bound the pv and pvc and if you find the error later you cannot bound pvc

  So 1st get pv yaml using k get pv -o yaml and remove the irrelavant field and apply pv and recreate the pvc

WaitForFirstCustomer means if you bound the pod only then it will be bound

Update

secretAsVolume

apiVersion: v1
kind: Pod
metadata:
  name: secret-test-pod
spec:
  containers:
    - name: test-container
      image: nginx
      volumeMounts:
        # name must match the volume name below
        - name: secret-volume
          mountPath: /etc/secret-volume
          readOnly: true
  # The secret data is exposed to Containers in the Pod through a Volume.
  volumes:
    - name: secret-volume
      secret:
        secretName: test-secret

configMapAsVolume
apiVersion: v1
kind: Pod
metadata:
  name: configmap-pod
spec:
  containers:
    - name: test
      image: busybox:1.28
      volumeMounts:
        - name: config-vol
          mountPath: /etc/config
  volumes:
    - name: config-vol
      configMap:
        name: log-config
        items:
          - key: log_level
            path: log_level


persistentVolumeClaim
apiVersion: v1
kind: Pod
metadata:
  name: task-pv-pod
spec:
  volumes:
    - name: task-pv-storage
      persistentVolumeClaim:
        claimName: task-pv-claim
  containers:
    - name: task-pv-container
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: task-pv-storage

EmptyDir
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: registry.k8s.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /cache
      name: cache-volume
  volumes:
  - name: cache-volume
    emptyDir:
      sizeLimit: 500Mi
    OR
    emptyDir: {}

Host Path
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: registry.k8s.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-pd
      name: test-volume
  volumes:
  - name: test-volume
    hostPath:
      # directory location on host
      path: /data
      # this field is optional
      type: Directory

```

# 2021 Updates

## Docker

```yaml
docker images (To see the list of images in cluster)

docker ps (To see the number of running containers)

docker ps -a (To see all the containers including stopped one)

docker build pathtodockerfile -t webapp-color

docker run -p hostport:containerport imagename

If you modify the dockerfile you have to build and run again

docker exec -it containerid bash

docker run python:3.6 cat /etc/*release*

all the commands like -t -p should be given before containername/containerid
```

## Kube Config

```yaml
Default kubeconfig is in the location /.kube/config

kube config has 3sections users,contexts and clusters

kube config also has current-context

k config use-context kubeconfig=pathtokubeconfig contextname
```

## RBAC

```yaml
Default location for manifests is /etc/kubernetes/manifests

k get roles

k get rolebindings

role will be bound to rolebinding

k auth can-i get po --as username or system:serviceaccount:namespace:serviceaccountname

k create role developer --verb=get,create,delete --resource=pods

k create rolebinding devuserbinding --role developer --user dev-user

k get po --as username

if you have created role and rolebinding in specific namespace remember to give -n namespace while checking --as command asshole

k get clusterrole

k get clusterrolebindings

remember a cluster role can be assigned to a rolebinding

ClusterRole + ClusterRoleBinding

ClusterRole + RoleBinding

Role + Rolebinding

Role + ClusterRoleBinding is not possible

Cluster role and Cluster Role bindings is a cluster wide

Role and Rolebindings are namespacescoped
```

## Admission Controller

```yaml
--runtime-config=resourcegroup/version inside /etc/kubernetes/manifests/kube-apisever.yaml
```

## Helm

```yaml
helm env

helm version

helm with --debug will give verbose output

helm search hub wordpress

helm search repo bitnami/apache

helm repo add bitnami bitnamiurl

helm repo list

helm list

helm install name reponame/chart (helm install bravo bitnami/drupal)

helm uninstall releasename (helm uninstall bravo)

helm pull bitnami/apache (This will download a folder in zip format)

helm pull --untar bitnami/apache (This will unzip the folder)
```

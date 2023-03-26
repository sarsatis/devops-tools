# devops-tools

# Jenkins
### To bring up jenkins use below command (Note Remember to create dockercred)

> kubectl create secret docker-registry dockercred --docker-server=https://index.docker.io/v1/ --docker-username=sarthaksatish --docker-password=***** --docker-email=sarthak8055@gmail.com

> helm install --create-namespace jenkins jenkins/




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

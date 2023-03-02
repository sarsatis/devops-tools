# devops-tools
All the devops tools



1. Deployed nginx - k apply -f nginx.yaml
2. Add this in ingress.yaml kubernetes.io/ingress.class: "nginx"
3. kustomize build argo-cd/overlays/production | k apply -f - 
4. export PASS=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)
5. export INGRESS_HOST=127.0.0.1
6. argocd login --insecure --username admin --password $PASS --grpc-web argo-cd.$INGRESS_HOST.nip.io
7. open http://argo-cd.$INGRESS_HOST.nip.io
8. k apply -f project.yaml
9. k apply -f apps.yaml 

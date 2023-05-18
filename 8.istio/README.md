# WIP
# Istio on Azure VM
> k create ns istio-system
> curl -L https://istio.io/downloadIstio | sh -
> cd istio-1.17.2
> export PATH=$PWD/bin:$PATH
> istioctl install --set profile=demo -y && kubectl apply -f samples/addons
>
> k create ns demo
>  kubectl label namespace demo istio-injection=enabled
>
> kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
>
> kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"
<title>Simple Bookstore App</title>

> istioctl dashboard kiali
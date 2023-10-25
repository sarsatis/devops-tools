# WIP
# Istio on Azure VM
```yaml
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

```


# Set the ingress IP and ports:
```yaml
> export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
> export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
> export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')
> export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
> echo "$GATEWAY_URL"

```

# To Delete istio system
```t
> kubectl delete -f samples/addons
> istioctl uninstall -y --purge
```

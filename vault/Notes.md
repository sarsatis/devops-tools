
# Vault using helm

# Vault Installation
![](vault/img/Installation.png

```t
> helm repo add hashicorp https://helm.releases.hashicorp.com

> helm pull hashicorp/vault --untar

> k create ns vault

> k exec -it vault-0 -- vault status

> k exec -it vault-0 -- /bin/sh

> vault operator init
> vault operator unseal and provide 3 keys
> vault login loginkey(hvs.HuJAgFMDijwXWPmU9k4BWtOO)
```

```t
k exec -it vault-0 -- /bin/sh
vault secrets enable -path=crds kv-v2
vault kv get crds/mysql
vault kv put crds/mysql username=root 
password=12345
vault kv delete crds/mysql


vault secrets enable -path=crds kv-v2

cat <<EOF > /home/vault/app-policy.hcl
path "crds/data/mongodb" {
capabilities = ["create", "update"]
}
path "crds/data/mysql" {
capabilities = ["read"]
}
EOF

vault policy write app /home/vault/app-policy.hcl

vault policy list

vault policy read app

export VAULT_TOKEN="$(vault token create -field token -policy=app)"
vault kv put crds/mysql username=root
export VAULT_TOKEN=ORIGINALTOKEN after above doesnt work

```

> vault auth enable kubernetes

```t
vault write auth/kubernetes/config \
token_reviewer_jwt="$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" \
kubernetes_host=https://${KUBERNETES_PORT_443_TCP_ADDR}:443 \
kubernetes_ca_cert=@/var/run/secrets/kubernetes.io/serviceaccount/ca.crt
```

```t
vault write auth/kubernetes/role/phpapp \
bound_service_account_names=app \
bound_service_account_namespaces=vault \
policies=app \
ttl=1h
```

> kubectl describe clusterrolebinding vault-server-binding
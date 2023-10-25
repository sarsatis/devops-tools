```yaml
argo list

argo submit hello-world-wf.yaml
argo submit 2.hello-world-wf.yaml -p message=sarthak -p sar=thak
argo submit 2.hello-world-wf.yaml --parameter-file params.yaml

argo logs @latest
argo logs @latest -f
argo logs hello-world-6c9nh

argo delete hello-world-mvt8q hello-world-wsms4

argo get @latest
argo get workflowname




```
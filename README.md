# Processus d'installation 

## Flux : 

```
export GITHUB_TOKEN="<token>"
flux bootstrap github --owner=jeanludo --repository=fluxtp4 --path=clusters/project --personal --private=false

# Deploying the UI
flux create source helm ww-gitops \
 --url=https://helm.gitops.weave.works \
 --export > ./clusters/project/weave-gitops-source.yml

flux create helmrelease ww-gitops \
 --source=HelmRepository/ww-gitops \
 --chart=weave-gitops \
 --values=./weave-gitops-values.yml \
 --export > ./clusters/project/weave-gitops-helmrelease.yaml

# Access to the UI
kubectl port-forward pod/ww-gitops-weave-gitops-6fb6f5fb57-skcmn 9001:9001 -n flux-system
```


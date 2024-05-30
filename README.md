# Processus d'installation 

## Flux : 

```
export GITHUB_TOKEN="<token>"
flux bootstrap github --owner=jeanludo --repository=fluxtp4 --path=clusters/project --personal --private=false

# Deploiment de l'UI
flux create source helm ww-gitops \
 --url=https://helm.gitops.weave.works \
 --export > ./clusters/project/weave-gitops-source.yml

flux create helmrelease ww-gitops \
 --source=HelmRepository/ww-gitops \
 --chart=weave-gitops \
 --values=./weave-gitops-values.yml \
 --export > ./clusters/project/weave-gitops-helmrelease.yaml

kubectl apply -f ./clusters/project/weave-gitops-source.yaml
kubectl apply -f ./clusters/project/weave-gitops-helmrelease.yaml

# Acceder a l'UI

kubectl get pods -n flux-system
kubectl port-forward pod/ww-gitops-weave-gitops-6fc66d8597-pcjfs 9001:9001 -n flux-system
```

## Nextcloud en utilisant Flux :

```
# Deploiment de NextCloud
flux create source helm nextcloud \
  --url=https://nextcloud.github.io/helm/ \
  --export > ./clusters/project/nextcloud-source.yaml

flux create helmrelease nextcloud \
  --source=HelmRepository/nextcloud \
  --chart=nextcloud \
  --target-namespace=nextcloud \
  --values=./nextcloud-values.yaml \
  --interval=1m \
  --export > ./clusters/project/nextcloud-helmrelease.yaml

kubectl apply -f ./clusters/project/nextcloud-source.yaml
kubectl apply -f ./clusters/project/nextcloud-helmrelease.yaml

flux reconcile source helm nextcloud -n flux-system
flux reconcile helmrelease nextcloud -n flux-system

# Acceder a Nextcloud
kubectl port-forward svc/nextcloud-nextcloud 8080:8080 -n nextcloud
```


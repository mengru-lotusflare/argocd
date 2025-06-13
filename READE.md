## Setup argocd

```shell
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
kubectl create namespace argocd
helm install argocd argo/argo-cd --namespace argocd
```

## Add cluster to Argocd

```shell
argocd cluster add $(kubectl config current-context)
```

## Create argocd project
```shell
kubectl apply -f cassandra-argocd-prj.yaml
```

## Create argocd application
```shell
kubectl apply -f cassandra-argocd-app.yaml
```

## Access the project
```shell
kubectl port-forward service/argocd-server -n argocd 8080:443
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

## Create image pull secret
```shell
kubectl create secret docker-registry ecr-secret \
  --docker-server=637423436902.dkr.ecr.ap-southeast-1.amazonaws.com \
  --docker-username=AWS \
  --docker-password=$(aws ecr get-login-password --region ap-southeast-1) \
  --namespace=com-cassandra
```

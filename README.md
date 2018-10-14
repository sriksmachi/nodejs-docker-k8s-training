# nodejs-docker-k8s-training
This repository contains L100 training material for containerizing NodeJS + MySQL apps with deployment on K8s

## Kubernetes Cluster Deployment on Azure. 

###Login and create cluster

```
az login --use-device-code
az aks create -n myaks -g contoso --node-count 2 -s Standard_A1

```

### Connect with Kubectl

```
az aks get-credentials -n myaks -g contoso --node-count 2 -s Standard_A1

```
 ### Browse k8s dashboard
 ```
az aks browse -n myaks -g contoso --node-count 2 -s Standard_A1
```
### Delete Cluster
```
az aks delete -n myaks -g contoso --yes
```



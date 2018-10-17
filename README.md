# nodejs-docker-k8s-training
This repository contains L100 training material for containerizing NodeJS + MySQL apps with deployment on K8s

## Kubernetes Cluster Deployment on Azure. 

Ensure Resource group with name contoso is created at your preferred location. 
PS: myaks is the name of the cluster

### Login and create cluster

```
az login --use-device-code
az group create -l westus -n contoso
az aks create -n myaks -g contoso --node-count 2 -s Standard_A1
```
#### Create cluster with RBAC enabled
Please follow the steps in the below link and update the values accordingly.
https://docs.microsoft.com/en-us/azure/aks/aad-integration

az aks create -g contoso -n myaks \
  --generate-ssh-keys \
  --aad-server-app-id c9c2c195-d7fd-4564-8e91-dc39437d2b28 \
  --aad-server-app-secret jtw4RAPVL+zDm+oCxCiKi+Ngx3Mylctr5QoIjWwWo5g= \
  --aad-client-app-id e7946b29-c83c-42fa-b163-eb89653f1f12 \
  --aad-tenant-id 8f348a80-a372-4ace-a595-b2bd09210a68

### Connect with Kubectl

```
az aks get-credentials -n myaks -g contoso 
```
If the cluster is RBAC enabled run the below command
az aks get-credentials -n myaks -g contoso --admin

### Browse k8s dashboard
PS: Follow the instructions from link below for enabling RBAC, this is mandatory for browsing kubernetes portal. 
https://docs.microsoft.com/en-us/azure/aks/aad-integration
```
az aks browse -n myaks -g contoso 
```

### Delete Cluster
```
az aks delete -n myaks -g contoso --yes
```

## Minikube
Run K8s locally using https://kubernetes.io/docs/setup/minikube/

## K8s deployment on AWS (needs a purchased/free domain with provision to configure DNS)
```
kops create cluster --name=kubernetes.techtalkswithsriks.com --state=s3://kops-state-a411 --zones=eu-west-1a --node-count=2 --node-size=t2.micro --master-size=t2.micro --dns-zone=kubernetes.techtalkswithsriks.com

kops delete cluster --name=kubernetes.techtalkswithsriks.com --state=s3://kops-state-a411 --yes

kops update cluster --name=kubernetes.techtalkswithsriks.com --state=s3://kops-state-a411 --yes
```

# Deploying Application on Kubernetes
The YAML files are under the deploy folder.
### Deploy database
```
kubectl create -f .\mysql-deployment.yml
kubectl create -f .\mysql-service.yml

-- grab IP
kubectl get svc
```
### Database preparation
Connect from MySQL Workbench or any command line tool and run the below query

```
create database authorsdb;
show databases;
use authorsdb;
create table authors (Name text, PageCount integer, PublishedYear integer, BookName text);
insert into authors values ('Michael Wolff', 336, 2018, 'Fire and Fury');
insert into authors values ('Bob Woodward', 448, 2018, 'Fear: Trump in the White House');
insert into authors values ('Rachel Hollis', 336, 2018, 'Girl, Wash Your Face');
select * from authors
```
### Build Docker Image
Ensure you're under app folder
```
docker build -t vishwanathsrikanth/node-web-app .
--Replace the host and password values below
docker run -p 8080:3000 -d -e Host=13.76.7.30 -e Password='password' --name node-web-app vishwanathsrikanth/node-web-app
```
## Deploy app on Kubernetes
Open app.deployment.yml and update the MySQL IP address obtained from above
```
kubectl create -f .\app-deployment.yml
kubectl create -f .\app-service.yml
-- grab IP
kubectl get svc
curl IP:8080

Scale Pods
kubectl scale deployment nodewebapp --replicas=10


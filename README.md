# nodejs-docker-k8s-training
This repository contains L100 training material for containerizing NodeJS + MySQL apps with deployment on K8s

## Kubernetes Cluster Deployment on Azure. 

Ensure Resource group with name contoso is created at your preferred location. 
PS: myaks is the name of the cluster

### Login and create cluster

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

## Database preparation
```
create database authorsdb;
show databases;
use authorsdb;
create table authors (Name text, PageCount integer, PublishedYear integer, `Book Name` text);
insert into authors values ('Michael Wolff', 336, 2018, 'Fire and Fury');
insert into authors values ('Bob Woodward', 448, 2018, 'Fear: Trump in the White House');
insert into authors values ('Rachel Hollis', 336, 2018, 'Girl, Wash Your Face');
select * from authors
```

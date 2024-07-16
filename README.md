# Superset Deployment on Kubernetes with ClickHouse, PostgreSQL, and Redis

This guide walks you through deploying Superset along with ClickHouse on Kubernetes using Minikube. Below are the steps to get started:

## Prerequisites

- Kubernetes knowledge and Minikube installed on your local machine.
- Docker knowledge.
- Basic understanding of ClickHouse and Superset.

## Setup Steps

### 1. Clone the repository

```
git clone https://github.com/NaitikSinghal/k8s-deployment
```

### Start Minikube Cluster on local

```
minikube start
```

### 2. Create Persistent Volume and Persistent Volume Claim for ClickHouse

First, create a Persistent Volume (PV) and Persistent Volume Claim (PVC) for ClickHouse with 10GiB of storage.

```
kubectl apply -f clickhouse/clickhouse-statefulset.yaml
kubectl apply -f clickhouse/clickhouse-pvc.yaml
kubectl apply -f clickhouse/clickhouse-service.yaml

```
Check if the clickhouse pod is running, Go inside the clickhouse container and create Username, Password, and a database with following command.

```
kubectl exec -it <clickhouse-pod-name> -- bash
```

```
CREATE DATABASE database_name;
```
```
CREATE USER username IDENTIFIED WITH plaintext_password BY 'password';
```

```
GRANT ALL ON database_name.* TO username;
```
Once this is done, the clickhouse uri will be something like this -> ```clickhousedb://username:password@clickhouse:9000/database_name```
Use this URI to connect it with superset.

### 3. Deploy superset service

```
kubectl apply -f superset/superset-deployment.yaml
kubectl apply -f superset/superset-service.yaml

```
Create a user-admin and password by running this command in steps,

```
kubectl exec -it <superset-pod-name> -- /bin/bash
```
```
superset fab create-admin
```

### 4. Check the pods and services 

```
kubectl get pods
```
```
kubectl get service
```

### 5. Access the superset service

```
minikube service superset-service --url
```

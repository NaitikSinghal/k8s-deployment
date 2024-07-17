# Superset Deployment on Kubernetes with ClickHouse, PostgreSQL, and Redis

This guide walks you through deploying Superset along with ClickHouse, PostgreSQL, and Redis on Kubernetes using Minikube. Below are the steps to get started:

## Prerequisites

- Kubernetes knowledge and Minikube installed on your local machine.
- Docker knowledge.
- Basic understanding of ClickHouse, Superset, PostgreSQL, and Redis.

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
CREATE DATABASE superset_db;
```
```
CREATE USER superset_user IDENTIFIED WITH plaintext_password BY 'superset_password';
```

```
GRANT ALL ON database_name.* TO superset_user;
```
Once this is done, the clickhouse uri will be something like this -> ```clickhousedb://superset_user:superset_password@clickhouse:9000/superset_db```
Use this URI to connect it with superset.
### 3. Deploy the postgres service 

```
kubectl apply -f postgres/postgres-deployment.yaml
kubectl apply -f postgres/postgres-service.yaml
```

### 4. Deploy redis service

```
kubectl apply -f redis/redis-deployment.yaml
kubectl apply -f redis/redis-service.yaml
```

### 5. Deploy superset service

```
kubectl apply -f superset/superset-deployment.yaml
kubectl apply -f superset/superset-service.yaml

```

### 6. Check the pods and services 

```
kubectl get pods
```
```
kubectl get service
```

### 7. Access the superset service

```
minikube service superset-service --url
```

### 8. If Superset shows 500:internal server error

Run this commands to create a admin-user for superset,

```
kubectl exec -it <superset pod> -- /bin/bash
```
```
superset fab create-admin
```
```
superset fab create-permissions
```
#### Don't forget to install this package for clickhouse connection

```
pip install clickhouse-connect
```

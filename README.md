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
### 3. Deploy the postgres service 

```
kubectl apply -f postgres/postgres-deployment.yaml
kubectl apply -f postgres/postgres-service.yaml
```
Check if the postgres pod is running, Go inside the postgres container and create a Username, Password, and a database 'superset_db' with following commands.

```
kubectl exec -it <postgres-pod-name> -- psql -U postgres
```
check your postgres pod name here with the below command and paste it in above command.
```
kubectl get pods
```
```
CREATE DATABASE superset_db;
CREATE USER superset_user WITH PASSWORD 'superset_password';
GRANT ALL PRIVILEGES ON DATABASE superset_db TO superset_user;
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



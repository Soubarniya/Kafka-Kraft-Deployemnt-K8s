
# Kafka KRaft Deployment on Kubernetes

This repository contains resources and configurations for deploying Kafka using the KRaft metadata mode on Kubernetes.

## Prerequisites
Before you begin, ensure you have the following installed:

- Kubernetes cluster (minikube, Docker Desktop)
- kubectl configured to manage the cluster


## Deployment Steps



### 1. Configure Kafka Parameters

Adjust Kafka configurations as needed for your deployment. This typically involves modifying statefulset.yaml or configmap.yaml

### 2. Deploy Kafka
Deploy Kafka with KRaft using the provided manifests 

```bash
  kubectl apply -f storageclass.yaml
```
```bash
  kubectl apply -f pv.yaml
```
```bash
  kubectl apply -f configmap.yaml
```
```bash
  kubectl apply -f statefulset.yaml
```
```bash
  kubectl apply -f service.yaml
```
### 3. Verify Deployment

Check the status of Kafka pods:

```bash
  kubectl get pods
```
Ensure all Kafka pods are running and ready before proceeding.

### 4. Test Kafka Deployment
Test the Kafka deployment with kcat. Follow the steps below to deploy kcat and use it to test Kafka:


  ```bash
  kubectl apply -f kcat-deployment.yaml
```

List the pods available in the cluster:
  ```bash
  kubectl get pods
```
 The following steps show how to create Kafka topics in production manually:

a) Locate Kafka pods and choose any of the pod replicas.

 Enter the chosen Kafka pod 

  ```bash
  kubectl exec --stdin --tty kafka-0 -- /bin/sh
```
b) Execute the following to create a new topic

 ```bash
  kafka-topics --bootstrap-server kafka-headless:9092  --topic test --create --partitions 3 
```

c) Now to check whether the created topic is present in the other pods:

To list the topic from kafka-1 pod :
 ```bash
  kubectl exec --stdin --tty kafka-1  -- /bin/sh -c "kafka-topics --list --bootstrap-server kafka-1.kafka-headless.default.svc.cluster.local:9092" 
```



 To list the topic from kafka-2 pod:
 ```bash
  kubectl exec --stdin --tty kafka-2  -- /bin/sh -c "kafka-topics --list --bootstrap-server kafka-2.kafka-headless.default.svc.cluster.local:9092" 
```

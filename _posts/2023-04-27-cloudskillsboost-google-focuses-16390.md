---
layout: post
title: Optimize Costs for Google Kubernetes Engine - Challenge Lab - 16390
date: 2023-04-27 06:14:27 +0700
categories: [cloud]
tags: [lab, gcp]
---

Optimize Costs for Google Kubernetes Engine: Challenge Lab

## Task 1. Create our cluster and deploy our app

- Set default compute/zone

```
gcloud config set compute/zone us-central1-a
```

- Create a cluster

```
gcloud container clusters create cluster-name-change-me --num-nodes=2 --machine-type=n1-standard-2  --release-channel=rapid

```

- Create namespaces

```
kubectl create namespace dev && kubectl create namespace prod
```

## Task 2. Migrate to an optimized node pool

- List all node pool

```
gcloud container node-pools list --cluster=cluster-name-change-me
```

- Create a new node pool

```
gcloud container node-pools create node-pool-change-me \
  --cluster=cluster-name-change-me \
  --machine-type=custom-2-3584 \
  --num-nodes=2
```

- Cordoning off and Draining `default-pool`.
  - Cordon the existing node pool
  ```
  for node in $(kubectl get nodes -l cloud.google.com/gke-nodepool=node-pool-change-me -o=name); do kubectl cordon "$node"; done
  ```
  - Drain the existing node pool
  ```
  for node in $(kubectl get nodes -l cloud.google.com/gke-nodepool=node-pool-change-me -o=name); do kubectl drain --force --ignore-daemonsets --delete-local-data --grace-period=10 "$node" ; done
  ```
- Checking pods running on new node-pool

```
kubectl get pods --namespace dev -o wide
```

- Delete old node-pool

```
gcloud container node-pools delete default-pool --cluster hello-demo-cluster --zone us-central1-a
```

## Task 3. Apply a frontend update

- Create a pod disruption budget

```
kubectl create poddisruptionbudget gb-pdb-change-me --namespace dev --selector app=frontend --min-available 1
```

- Update Image version: [Source file on Github](https://github.com/GoogleCloudPlatform/microservices-demo/blob/main/release/kubernetes-manifests.yaml#L258)
- Add a newline of `imagePullPolicy:Always` next to `image:` tag
- Set edit user `export KUBE_EDITOR="nano"`
- Describe deployment: `kubectl describe deployments`
- Edit deployment: `kubectl edit deployment/nginx-deployment`
- Apply changes: `kubectl apply -f ./release/kubernetes-manifests.yaml --namespace dev`

## Task 4. Autoscale from estimated traffic

- In Cloud Shell, run this command to inspect your cluster's deployments:

```
kubectl get deployments
```

- Apply horizontal autoscaling to the `php-apache` deployment:

```
kubectl autoscale deployment php-apache --namespace dev --cpu-percent=50 --min=1 --max=10
```

- Enable autoscaling for your cluster:

```
gcloud beta container clusters update cluster-name-change-me --enable-autoscaling --min-nodes 1 --max-nodes 5
```

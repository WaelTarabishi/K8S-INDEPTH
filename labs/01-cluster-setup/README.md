# Lab 01 - Kind Cluster Setup

## Objective

Create a local multi-node Kubernetes cluster using Kind.

---

## Prerequisites

- Docker Desktop
- kubectl
- kind

---

## Create Cluster

```bash
kind create cluster --name learning --config kind-config.yaml
```

---

## Verify

```bash
kubectl cluster-info
kubectl get nodes
```

---

## Learning Outcomes

- Understand what Kind is
- Understand Kubernetes Nodes
- Learn how kubectl communicates with the API Server

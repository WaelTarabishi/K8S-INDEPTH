# Lab 04 - ReplicaSets

## Objective

Learn how a ReplicaSet keeps a fixed number of Pod replicas running. This lab focuses on labels, selectors, reconciliation, and basic scaling behavior.

## Theory

A ReplicaSet is a controller that ensures a target number of matching Pods exists at all times. It uses a label selector to decide which Pods it owns. If a managed Pod disappears, the ReplicaSet creates a replacement. ReplicaSets are important to understand because Deployments use them under the hood, but in day-to-day work you usually manage Deployments rather than standalone ReplicaSets.

## Prerequisites

- A running Kubernetes cluster
- `kubectl`
- Working knowledge of Pods, labels, and YAML

## Lab Steps

1. Create a namespace for the lab:
   - `kubectl create namespace replicaset-lab`
2. Create a ReplicaSet manifest with a selector such as `app: rs-demo` and a replica count of `3`.
3. Apply the manifest:
   - `kubectl apply -f replicaset.yaml -n replicaset-lab`
4. Inspect the ReplicaSet and the Pods it owns:
   - `kubectl get rs -n replicaset-lab`
   - `kubectl get pods -n replicaset-lab --show-labels`
5. Delete one managed Pod and observe reconciliation:
   - `kubectl delete pod <pod-name> -n replicaset-lab`
6. Scale the ReplicaSet and verify the new replica count:
   - `kubectl scale rs rs-demo --replicas=5 -n replicaset-lab`

## Verification

- `kubectl get rs -n replicaset-lab`
- `kubectl describe rs rs-demo -n replicaset-lab`
- `kubectl get pods -n replicaset-lab --show-labels`
- `kubectl get events -n replicaset-lab --sort-by=.metadata.creationTimestamp`

## What I Learned

Expected outcomes after completing this lab:

- I can explain how reconciliation keeps the desired number of Pods running.
- I understand why labels and selectors must be designed carefully.
- I can describe the relationship between ReplicaSets and Deployments.

## Interview Questions

1. What problem does a ReplicaSet solve?
2. How does a ReplicaSet know which Pods to manage?
3. What happens if you delete a Pod owned by a ReplicaSet?
4. Why are ReplicaSets usually created indirectly through Deployments?
5. What risk is introduced by a poorly chosen label selector?

## Common Mistakes

- Creating a selector that matches unintended Pods
- Editing Pod labels manually and accidentally changing ownership
- Using a standalone ReplicaSet when rollout and rollback behavior is needed
- Forgetting to inspect events when scaling does not behave as expected

## References

- ReplicaSet: https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
- Labels and Selectors: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
- Controllers: https://kubernetes.io/docs/concepts/architecture/controller/

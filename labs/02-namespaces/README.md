# Lab 02 - Namespaces

## Objective

Learn how namespaces provide logical separation inside a Kubernetes cluster. This lab focuses on creating namespaces, placing workloads inside them, and distinguishing namespaced resources from cluster-scoped resources.

## Theory

Namespaces partition many Kubernetes resources within the same cluster. They are commonly used to separate teams, environments, or applications without needing a separate cluster for each one. Most workload objects such as Pods, Deployments, Services, ConfigMaps, and Secrets are namespaced. Some resources, such as Nodes, PersistentVolumes, and Namespaces themselves, are cluster-scoped. Namespaces improve organization and are often used together with RBAC, quotas, and network policies.

## Prerequisites

- A running Kubernetes cluster
- `kubectl` configured for that cluster
- Basic understanding of common Kubernetes objects

## Lab Steps

1. Review the namespaces that already exist:
   - `kubectl get namespaces`
2. Create a dedicated practice namespace:
   - `kubectl create namespace namespaces-lab`
3. Create a simple test Pod in that namespace:
   - `kubectl run demo --image=nginx:1.25 --restart=Never -n namespaces-lab`
4. Query resources inside the namespace:
   - `kubectl get pods -n namespaces-lab`
   - `kubectl get all -n namespaces-lab`
5. Compare namespaced resources with cluster-scoped resources:
   - `kubectl get nodes`
   - `kubectl get namespaces`
6. Optionally make the namespace the default for the current context:
   - `kubectl config set-context --current --namespace=namespaces-lab`

## Verification

- `kubectl get namespaces`
- `kubectl describe namespace namespaces-lab`
- `kubectl get all -n namespaces-lab`
- `kubectl get nodes`

## What I Learned

Expected outcomes after completing this lab:

- I can explain why namespaces are useful in shared clusters.
- I know which common resources are namespaced and which are cluster-scoped.
- I can use `kubectl` flags or context settings to work in the correct namespace consistently.

## Interview Questions

1. What problem do namespaces solve in Kubernetes?
2. Which Kubernetes resources are cluster-scoped?
3. How do namespaces relate to RBAC?
4. What is the difference between the `default` namespace and a custom namespace?
5. How do you make `kubectl` use a namespace by default?

## Common Mistakes

- Forgetting the `-n` flag and creating resources in the `default` namespace by accident
- Assuming namespaces provide full security isolation by themselves
- Confusing cluster-scoped resources with namespaced resources
- Reusing the same labels and names across labs without checking the active namespace

## References

- Namespaces: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
- Namespaces Walkthrough: https://kubernetes.io/docs/tasks/administer-cluster/namespaces-walkthrough/
- RBAC Authorization: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

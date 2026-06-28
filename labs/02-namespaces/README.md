# Lab 02 - Namespaces

## Objective

Learn how namespaces organize Kubernetes resources and practice creating, using, and deleting a namespace named `learning`.

## Prerequisites

- A running Kubernetes cluster from Lab 01
- `kubectl` configured to talk to that cluster
- Permission to create namespaces

## Key Concepts

- Namespaces provide logical separation inside a single cluster.
- Most workload resources such as Pods, Deployments, Services, ConfigMaps, and Secrets are namespaced.
- Some resources such as Nodes, Namespaces, and PersistentVolumes are cluster-scoped.
- Setting a default namespace in the current context reduces repeated use of the `-n` flag.

## Lab Steps

1. List the namespaces that already exist.
   - What we are doing: Review the built-in namespaces before adding our own.
   - Command:
     ```bash
     kubectl get namespaces
     ```
   - Short explanation: Shows all namespaces currently available in the cluster.

2. Create a namespace imperatively.
   - What we are doing: Create `learning` with a direct CLI command.
   - Command:
     ```bash
     kubectl create namespace learning
     ```
   - Short explanation: Creates the namespace immediately without a manifest file.

3. Delete the namespace so we can recreate it declaratively.
   - What we are doing: Reset the lab so both creation approaches are clear.
   - Command:
     ```bash
     kubectl delete namespace learning
     ```
   - Short explanation: Removes the namespace and everything inside it.

4. Review the declarative manifest.
   - What we are doing: Check the reusable namespace YAML file for this lab.
   - Command:
     ```bash
     kubectl apply --dry-run=client -f manifests/namespaces/namespace.yaml
     ```
   - Short explanation: Validates the namespace manifest locally before applying it.

5. Create the namespace declaratively.
   - What we are doing: Apply the reusable manifest from `manifests/namespaces/namespace.yaml`.
   - Command:
     ```bash
     kubectl apply -f manifests/namespaces/namespace.yaml
     ```
   - Short explanation: Creates or updates the namespace from YAML.

6. Switch the current context to use `learning` by default.
   - What we are doing: Make `kubectl` target the practice namespace automatically.
   - Command:
     ```bash
     kubectl config set-context --current --namespace=learning
     ```
   - Short explanation: Updates the current kubeconfig context to use `learning` as the default namespace.

7. List resources in the namespace.
   - What we are doing: Verify that the namespace is selectable and currently empty.
   - Command:
     ```bash
     kubectl get all -n learning
     ```
   - Short explanation: Lists common namespaced resources in `learning`.

8. Reset the context namespace if needed.
   - What we are doing: Return the current context to the `default` namespace after the lab.
   - Command:
     ```bash
     kubectl config set-context --current --namespace=default
     ```
   - Short explanation: Prevents later commands from accidentally targeting `learning`.

## YAML Examples

Main manifest for this lab:

- `manifests/namespaces/namespace.yaml`

Small example:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: learning
```

## Verification

```bash
kubectl get namespaces
kubectl describe namespace learning
kubectl config view --minify | findstr namespace
kubectl get all -n learning
```

## Expected Output

- `kubectl get namespaces` should include `learning`.
- `kubectl describe namespace learning` should show namespace details and events.
- `kubectl get all -n learning` should return no workloads unless you already created some.
- `kubectl config view --minify | findstr namespace` should show the active default namespace if you changed the context.

## Troubleshooting

- `namespaces "learning" already exists`:
  The namespace is already present, so use `kubectl get namespace learning` or re-apply the manifest.
- Resources appear in `default` instead of `learning`:
  Add `-n learning` or update the context namespace with `kubectl config set-context --current --namespace=learning`.
- Cannot delete the namespace:
  Check for workloads or finalizers keeping the namespace in `Terminating`.
- Dry run fails:
  Confirm the YAML indentation and that the file path is correct.

## Cleanup

```bash
kubectl config set-context --current --namespace=default
kubectl delete -f manifests/namespaces/namespace.yaml
```

## Key Takeaways

- Namespaces help organize and isolate workloads inside one cluster.
- `kubectl create namespace` is quick, but YAML is better for repeatable setup.
- Context namespace changes are convenient but easy to forget, so verify them often.

## Interview Questions

1. What problem do namespaces solve in Kubernetes?
   They organize and separate resources within the same cluster.
2. Are namespaces cluster-scoped or namespaced resources?
   Namespaces themselves are cluster-scoped resources.
3. Which common resources are namespaced?
   Pods, Deployments, Services, ConfigMaps, and Secrets are namespaced.
4. How do you make `kubectl` use a namespace by default?
   Run `kubectl config set-context --current --namespace=<namespace-name>`.

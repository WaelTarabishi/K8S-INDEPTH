# Lab 06 - Services

## Objective

Learn why Services are needed, how selectors connect them to Pods, and how to expose a Deployment with `ClusterIP` and `NodePort`.

## Prerequisites

- A running Kubernetes cluster
- `kubectl`
- The `learning` namespace from Lab 02
- The nginx Deployment from Lab 05 or the ability to apply it again

## Key Concepts

- Pod IP addresses are not stable enough to be used directly by clients.
- A Service gives a stable virtual IP and DNS name to a set of Pods.
- `ClusterIP` is for internal cluster access.
- `NodePort` exposes the Service on a port on each node for simple external testing.

## Lab Steps

1. Apply the backend Deployment if it is not already running.
   - What we are doing: Make sure the Service has Pods to target.
   - Command:
     ```bash
     kubectl apply -f manifests/deployments/deployment.yaml
     ```
   - Short explanation: Creates the nginx Deployment used by the Service selectors.

2. Validate the ClusterIP Service manifest.
   - What we are doing: Review the internal-only Service definition.
   - Command:
     ```bash
     kubectl apply --dry-run=client -f manifests/services/clusterip-service.yaml
     ```
   - Short explanation: Checks the ClusterIP manifest before applying it.

3. Apply the ClusterIP Service.
   - What we are doing: Expose the nginx Deployment inside the cluster.
   - Command:
     ```bash
     kubectl apply -f manifests/services/clusterip-service.yaml
     ```
   - Short explanation: Creates a stable internal endpoint for Pods labeled `app: nginx`.

4. Validate and apply the optional NodePort Service.
   - What we are doing: Create a simple external test path for local clusters.
   - Command:
     ```bash
     kubectl apply --dry-run=client -f manifests/services/nodeport-service.yaml
     kubectl apply -f manifests/services/nodeport-service.yaml
     ```
   - Short explanation: Exposes the same backend on a node port for browser or curl testing from outside the cluster.

5. Inspect Services and endpoints.
   - What we are doing: Confirm the selectors matched the backend Pods.
   - Command:
     ```bash
     kubectl get svc,endpoints -n learning
     ```
   - Short explanation: Shows the Services and the backend Pod IPs selected for each one.

6. Test Service connectivity from inside the cluster.
   - What we are doing: Call the ClusterIP Service from a temporary test Pod.
   - Command:
     ```bash
     kubectl run test-client --image=busybox:1.36 --restart=Never -it --rm -n learning -- wget -qO- http://nginx-service
     ```
   - Short explanation: Launches a temporary Pod and sends an HTTP request to the Service DNS name.

7. Review the Service DNS names.
   - What we are doing: Understand how Kubernetes DNS resolves Services.
   - Command:
     ```bash
     kubectl get svc nginx-service -n learning
     ```
   - Short explanation: Helps connect the Service name to the in-cluster DNS form such as `nginx-service.learning.svc.cluster.local`.

## YAML Examples

Main manifests for this lab:

- `manifests/services/clusterip-service.yaml`
- `manifests/services/nodeport-service.yaml`

Small example:

```yaml
spec:
  selector:
    app: nginx
```

## Verification

```bash
kubectl get svc -n learning
kubectl describe svc nginx-service -n learning
kubectl get endpoints nginx-service -n learning
kubectl get pods -n learning -l app=nginx -o wide
```

## Expected Output

- `kubectl get svc -n learning` should show `nginx-service` with a ClusterIP.
- `kubectl get endpoints nginx-service -n learning` should list Pod IPs and port `80`.
- The temporary test Pod should return the nginx default HTML page.
- If the NodePort Service is applied, `kubectl get svc -n learning` should show a node port in the `30080` range.

## Troubleshooting

- Service has no endpoints:
  Check that the selector labels match the Deployment Pod labels.
- DNS lookup fails:
  Verify CoreDNS is running in `kube-system`.
- NodePort is unreachable:
  Confirm the local cluster exposes node ports the way your platform expects.
- Wrong port:
  Check the difference between `port`, `targetPort`, and `nodePort`.

## Cleanup

```bash
kubectl delete -f manifests/services/nodeport-service.yaml
kubectl delete -f manifests/services/clusterip-service.yaml
```

## Key Takeaways

- Services provide stable access to changing Pods.
- Selectors connect a Service to its backend Pods.
- Cluster DNS lets Pods call Services by name instead of IP address.

## Interview Questions

1. Why are Services needed if Pods already have IP addresses?
   Pod IPs can change, but Services provide a stable endpoint.
2. What is the default Service type in Kubernetes?
   `ClusterIP`.
3. What does a Service selector do?
   It matches the Pods that should receive traffic.
4. What is the in-cluster DNS form of a Service?
   It follows the pattern `<service>.<namespace>.svc.cluster.local`.

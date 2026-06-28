# Lab 11 - Ingress

## Objective

Learn how Kubernetes Ingress exposes HTTP and HTTPS applications through a single routing layer in front of Services.

## Theory

Ingress is an API object that defines HTTP and HTTPS routing rules such as host-based and path-based matching. An Ingress resource does nothing by itself; it requires an ingress controller to watch the object and program a data plane. Ingress sits above Services: Services provide stable backend access inside the cluster, while Ingress provides user-facing routing into those Services. TLS termination is commonly handled at the Ingress layer.

## Prerequisites

- A running Kubernetes cluster
- `kubectl`
- An installed ingress controller
- A backend Deployment and Service to route traffic to
- Basic understanding of Services and DNS

## Lab Steps

1. Verify that an ingress controller is installed:
   - `kubectl get pods -A | findstr ingress`
2. Create a namespace and backend workload:
   - `kubectl create namespace ingress-lab`
   - `kubectl create deployment web --image=nginx:1.25 -n ingress-lab`
   - `kubectl expose deployment web --port=80 --target-port=80 --name=web-svc -n ingress-lab`
3. Create an Ingress resource that routes `demo.local` to `web-svc`.
4. Apply the Ingress and inspect the assigned address or controller status:
   - `kubectl get ingress -n ingress-lab`
5. Test host-based routing with a request that sets the `Host` header:
   - `curl -H "Host: demo.local" http://<ingress-address>`
6. If your controller supports it and you have a certificate Secret, add a TLS section and retest with HTTPS.

## Verification

- `kubectl get ingress -n ingress-lab`
- `kubectl describe ingress -n ingress-lab`
- `kubectl get svc -n ingress-lab`
- `curl -H "Host: demo.local" http://<ingress-address>`

## What I Learned

Expected outcomes after completing this lab:

- I can explain the difference between a Service and an Ingress.
- I understand that an ingress controller is required for the resource to work.
- I know how host-based and path-based routing are defined at a high level.

## Interview Questions

1. What problem does Ingress solve?
2. Why is an ingress controller required?
3. How is Ingress different from a Service of type `LoadBalancer`?
4. What is TLS termination in the context of Ingress?
5. What information would you check first if an Ingress is not routing traffic?

## Common Mistakes

- Creating an Ingress without installing a controller
- Testing routing without setting the expected `Host` header
- Assuming Ingress can route arbitrary TCP or UDP traffic
- Forgetting that the backend Service must already be reachable inside the cluster

## References

- Ingress: https://kubernetes.io/docs/concepts/services-networking/ingress/
- Ingress Controllers: https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/
- Ingress NGINX Documentation: https://kubernetes.github.io/ingress-nginx/

# Lab 13 - Gateway API

## Objective

Explore the Gateway API resource model and understand how it offers a more expressive and role-oriented approach to traffic management than classic Ingress.

## Theory

Gateway API is a Kubernetes project that defines resources such as `GatewayClass`, `Gateway`, and `HTTPRoute` for managing traffic. It separates infrastructure ownership from application routing ownership more clearly than Ingress. A platform team can manage shared gateways while application teams attach routes to them. Like Ingress, Gateway API still depends on a controller implementation, but it provides richer status, attachment, and policy patterns.

## Prerequisites

- A running Kubernetes cluster
- Gateway API CRDs installed
- A Gateway API-compatible controller installed
- `kubectl`
- A backend Deployment and Service to route traffic to

## Lab Steps

1. Verify that the Gateway API CRDs exist:
   - `kubectl get crd gateways.gateway.networking.k8s.io`
2. Confirm that a compatible controller is installed and note the `GatewayClass` it provides:
   - `kubectl get gatewayclass`
3. Create a namespace and backend workload:
   - `kubectl create namespace gateway-lab`
   - `kubectl create deployment web --image=nginx:1.25 -n gateway-lab`
   - `kubectl expose deployment web --port=80 --target-port=80 --name=web-svc -n gateway-lab`
4. Create a `Gateway` that listens for HTTP traffic.
5. Create an `HTTPRoute` that attaches to the Gateway and forwards traffic to `web-svc`.
6. Test routing and inspect route status conditions such as `Accepted` and `ResolvedRefs`.

## Verification

- `kubectl get gatewayclass`
- `kubectl get gateways -n gateway-lab`
- `kubectl get httproute -n gateway-lab`
- `kubectl describe httproute -n gateway-lab`

## What I Learned

Expected outcomes after completing this lab:

- I can explain the roles of `GatewayClass`, `Gateway`, and `HTTPRoute`.
- I understand why Gateway API separates infrastructure and application concerns more clearly than Ingress.
- I know how to verify attachment and status when traffic is not flowing.

## Interview Questions

1. What problem is Gateway API trying to solve compared to Ingress?
2. What is the difference between a `GatewayClass` and a `Gateway`?
3. What does an `HTTPRoute` represent?
4. Why is status inspection especially important with Gateway API?
5. How can Gateway API support clearer separation of responsibilities between platform and application teams?

## Common Mistakes

- Assuming Gateway API works without installing a compatible controller
- Creating a `GatewayClass` manually when the controller already provides one
- Ignoring route and gateway status conditions during troubleshooting
- Treating Gateway API objects as portable without checking controller-specific support

## References

- Gateway API Documentation: https://gateway-api.sigs.k8s.io/
- Kubernetes Service APIs Overview: https://gateway-api.sigs.k8s.io/concepts/api-overview/
- Gateway API Implementations: https://gateway-api.sigs.k8s.io/implementations/

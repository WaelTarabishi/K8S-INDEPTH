# Lab 12 - Helm

## Objective

Use Helm to package, install, upgrade, and roll back Kubernetes applications with reusable templates and values.

## Theory

Helm is a package manager for Kubernetes. A chart is a collection of templates, default values, and metadata that renders into Kubernetes manifests. A release is a deployed instance of a chart in a cluster. Helm is useful when an application has repeated resource patterns, environment-specific values, or shared deployment conventions. It reduces copy-paste YAML, but it also adds a templating layer that must still be understood and reviewed carefully.

## Prerequisites

- A running Kubernetes cluster
- Helm installed
- `kubectl`
- Basic understanding of standard Kubernetes manifests

## Lab Steps

1. Create a namespace for the lab:
   - `kubectl create namespace helm-lab`
2. Generate a starter chart:
   - `helm create webapp`
3. Review the chart structure:
   - `Chart.yaml`
   - `values.yaml`
   - `templates/`
4. Install the chart into the cluster:
   - `helm install webapp ./webapp -n helm-lab`
5. Override one or two values and perform an upgrade:
   - `helm upgrade webapp ./webapp -n helm-lab --set replicaCount=2`
6. Review release history and test rollback:
   - `helm history webapp -n helm-lab`
   - `helm rollback webapp 1 -n helm-lab`

## Verification

- `helm list -n helm-lab`
- `helm status webapp -n helm-lab`
- `helm get values webapp -n helm-lab`
- `kubectl get all -n helm-lab`

## What I Learned

Expected outcomes after completing this lab:

- I can explain what a chart and a release are in Helm.
- I understand how values drive manifest rendering.
- I can install, upgrade, inspect, and roll back a Helm release.

## Interview Questions

1. What is the difference between a Helm chart and a Helm release?
2. Why do teams use Helm instead of raw manifests in some cases?
3. What does `values.yaml` control?
4. How do you inspect the rendered state of a Helm release?
5. When would a rollback be useful?

## Common Mistakes

- Treating Helm as a substitute for understanding the underlying manifests
- Overusing complex template logic for simple configuration
- Upgrading a chart without checking what changed
- Forgetting that Helm tracks release history in the cluster

## References

- Helm Documentation: https://helm.sh/docs/
- Chart Template Guide: https://helm.sh/docs/chart_template_guide/
- Chart Best Practices: https://helm.sh/docs/chart_best_practices/

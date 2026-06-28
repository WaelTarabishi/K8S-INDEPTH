# Lab 10 - Volumes

## Objective

Understand how Kubernetes volumes handle data that should outlive a container process and how persistent storage differs from ephemeral storage.

## Theory

Container filesystems are usually ephemeral. If a container is replaced, local writes inside the container image layer are lost. Kubernetes volumes provide storage that can be shared by containers in a Pod or retained beyond container restarts. `emptyDir` is created when a Pod starts and removed when the Pod is deleted. Persistent storage is usually requested through a `PersistentVolumeClaim` and backed by a `StorageClass` that provisions or connects real storage.

## Prerequisites

- A running Kubernetes cluster
- `kubectl`
- Basic understanding of Pods
- A cluster with a default `StorageClass` if you want to test dynamic persistent storage

## Lab Steps

1. Create a namespace for the lab:
   - `kubectl create namespace volumes-lab`
2. Start with an `emptyDir` example to observe ephemeral behavior:
   - Create a Pod that mounts `emptyDir` at `/data`
   - Write a test file to `/data`
   - Delete the Pod and confirm the file is gone after recreation
3. Check whether the cluster has a default storage class:
   - `kubectl get storageclass`
4. If a storage class exists, create a `PersistentVolumeClaim` and mount it into a Pod or Deployment.
5. Write a file to the persistent mount, restart the Pod, and verify that the file still exists.
6. Compare the outcomes of `emptyDir` and persistent storage in your notes.

## Verification

- `kubectl get pv`
- `kubectl get pvc -n volumes-lab`
- `kubectl get storageclass`
- `kubectl exec -it <pod-name> -n volumes-lab -- ls /data`

## What I Learned

Expected outcomes after completing this lab:

- I can explain why container-local filesystem state is usually not enough.
- I understand the difference between ephemeral volumes and persistent storage.
- I know the roles of `PersistentVolume`, `PersistentVolumeClaim`, and `StorageClass`.

## Interview Questions

1. Why is container filesystem data usually not considered durable?
2. What is the difference between `emptyDir` and a `PersistentVolumeClaim`?
3. What does a `StorageClass` do?
4. When is a `PersistentVolume` cluster-scoped?
5. Why might a Pod remain pending when using a PVC?

## Common Mistakes

- Writing important data only to the container filesystem
- Assuming a local learning cluster always has a dynamic provisioner installed
- Ignoring access modes and storage class requirements
- Expecting `emptyDir` data to survive Pod deletion

## References

- Volumes: https://kubernetes.io/docs/concepts/storage/volumes/
- Persistent Volumes: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- Storage Classes: https://kubernetes.io/docs/concepts/storage/storage-classes/

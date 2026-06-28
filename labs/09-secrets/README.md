# Lab 09 - Secrets

## Objective

Learn how to store sensitive-looking placeholder data in Kubernetes Secrets and consume it as environment variables or mounted files.

## Prerequisites

- A running Kubernetes cluster
- `kubectl`
- The `learning` namespace from Lab 02
- Basic understanding of ConfigMaps

## Key Concepts

- Secrets are used for sensitive data such as passwords, tokens, and certificates.
- Secret data in YAML is usually base64-encoded, not encrypted.
- A Pod can consume Secret data as environment variables or mounted files.
- Kubernetes Secrets should still be protected with RBAC and encryption at rest when available.

## Lab Steps

1. Create a Secret imperatively from literal values.
   - What we are doing: Create a Secret quickly from the CLI.
   - Command:
     ```bash
     kubectl create secret generic app-secret --from-literal=DB_USER=demo-user --from-literal=DB_PASSWORD=change-me -n learning
     ```
   - Short explanation: Creates a Secret named `app-secret` with placeholder values.

2. Delete the imperatively created Secret.
   - What we are doing: Remove it so the YAML version uses the same name cleanly.
   - Command:
     ```bash
     kubectl delete secret app-secret -n learning
     ```
   - Short explanation: Cleans up the quick CLI-created Secret before applying manifests.

3. Review and apply the Secret manifest.
   - What we are doing: Create the reusable Secret from YAML.
   - Command:
     ```bash
     kubectl apply -f manifests/secrets/app-secret.yaml
     ```
   - Short explanation: Creates the Secret in the `learning` namespace.

4. Apply the Pod that uses Secret values as environment variables.
   - What we are doing: Test Secret injection into environment variables.
   - Command:
     ```bash
     kubectl apply -f manifests/secrets/secret-env-pod.yaml
     ```
   - Short explanation: Creates a Pod that reads Secret keys from env vars.

5. Apply the Pod that mounts Secret values as files.
   - What we are doing: Test Secret file mounts.
   - Command:
     ```bash
     kubectl apply -f manifests/secrets/secret-volume-pod.yaml
     ```
   - Short explanation: Creates a Pod that reads Secret keys from a mounted volume.

6. Verify the Secret object.
   - What we are doing: Inspect metadata without printing raw values.
   - Command:
     ```bash
     kubectl describe secret app-secret -n learning
     ```
   - Short explanation: Shows Secret metadata and keys without printing the decoded values by default.

7. Verify Secret environment variables carefully.
   - What we are doing: Confirm the Pod can see the secret-based variables.
   - Command:
     ```bash
     kubectl exec secret-env-pod -n learning -- printenv | findstr DB_USER
     ```
   - Short explanation: Checks the presence of secret-derived environment variables.

8. Verify mounted Secret files.
   - What we are doing: Check that the Secret keys exist as files.
   - Command:
     ```bash
     kubectl exec secret-volume-pod -n learning -- ls /etc/app-secret
     ```
   - Short explanation: Lists files created from the Secret volume mount.

9. Review the base64 note.
   - What we are doing: Understand how Secret values appear in YAML.
   - Command:
     ```bash
     kubectl get secret app-secret -n learning -o yaml
     ```
   - Short explanation: Shows the base64-encoded values stored under the `data` field.

## YAML Examples

Main manifests for this lab:

- `manifests/secrets/app-secret.yaml`
- `manifests/secrets/secret-env-pod.yaml`
- `manifests/secrets/secret-volume-pod.yaml`

Small example:

```yaml
data:
  DB_USER: ZGVtby11c2Vy
```

## Verification

```bash
kubectl get secrets -n learning
kubectl describe secret app-secret -n learning
kubectl exec secret-env-pod -n learning -- printenv | findstr DB_USER
kubectl exec secret-volume-pod -n learning -- ls /etc/app-secret
```

## Expected Output

- `kubectl get secrets -n learning` should show `app-secret`.
- `kubectl describe secret app-secret -n learning` should list the keys without printing the actual values.
- The env-based Pod should expose `DB_USER`.
- The volume-based Pod should contain files created from the Secret keys.

## Troubleshooting

- Pod cannot find the Secret:
  Make sure the Secret exists in the same namespace as the Pod.
- Secret values look readable after decoding:
  That is expected because base64 is encoding, not encryption.
- `kubectl exec` shows no environment variable:
  Confirm the `secretKeyRef` field points to the correct Secret name and key.
- Mounted files are missing:
  Check the Secret volume and mount path configuration.

## Cleanup

```bash
kubectl delete -f manifests/secrets/secret-volume-pod.yaml
kubectl delete -f manifests/secrets/secret-env-pod.yaml
kubectl delete -f manifests/secrets/app-secret.yaml
```

## Key Takeaways

- Secrets are for sensitive data, while ConfigMaps are for non-sensitive configuration.
- Secret values in manifests are encoded, not encrypted by default.
- RBAC and cluster security settings still matter when using Secrets.

## Interview Questions

1. What is the difference between a Secret and a ConfigMap?
   Secrets are intended for sensitive data, while ConfigMaps are for non-sensitive configuration.
2. Is base64 encoding a security control?
   No, it is only an encoding format.
3. How can a Pod consume Secret data?
   It can use environment variables or mounted files.
4. Why is RBAC important for Secrets?
   It limits who can read or modify sensitive cluster data.
5. What security warning applies to Kubernetes Secrets by default?
   They are encoded, not encrypted, unless extra cluster protections are enabled.

## Helm Upgrade Error: Resource Already Exists

```bash
$ helm upgrade data-service ./charts/data-service --namespace application

Error: UPGRADE FAILED: rendered manifests contain a resource that already exists.
Unable to continue with update: ServiceAccount "data-service-sa" in namespace "application" exists and cannot be imported into the current release:
invalid ownership metadata;
label validation error: missing key "app.kubernetes.io/managed-by": must be set to "Helm";
annotation validation error: missing key "meta.helm.sh/release-name": must be set to "data-service";
annotation validation error: missing key "meta.helm.sh/release-namespace": must be set to "application"
```
- Cause: A resource (e.g., ServiceAccount) already exists but was not created by Helm, so Helm cannot manage it.
- Fix:
    - Option 1: Delete the existing resource (if safe to do so)
    - Adopt the resource using helm.sh/resource-policy: keep or manually add Helm labels/annotations (not recommended for critical workloads).

## FailedScheduling – Insufficient Memory
```bash
Warning  FailedScheduling  default-scheduler
0/1 nodes are available: 1 Insufficient memory.
preemption: 0/1 nodes are available: 1 No preemption victims found for incoming pod.
```
- Cause: The pod requires more memory than what is available on the node.
- Fix:
    - Lower memory requests/limits in your deployment YAML.
    - Add more memory to nodes or scale the node pool.
    - Use resource requests/limits appropriately.

## CrashLoopBackOff
```bash
kubectl get pods
NAME                           READY   STATUS             RESTARTS   AGE
my-app-5f4d78b4f8-fj5rt        0/1     CrashLoopBackOff   5          3m
```
- Cause: The container crashes repeatedly due to:
    - Misconfigured environment variables
    - Application startup errors
- Fix:
    - Inspect logs: `kubectl logs <pod-name>`
    - Validate health checks (readiness/liveness probes).

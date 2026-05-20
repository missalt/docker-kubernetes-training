# Exercise 14: RBAC and Service Accounts

In Kubernetes, every Pod runs under a **ServiceAccount**. By default that account has very limited permissions. Using **Roles** and **RoleBindings**, you control exactly what it can access.

In this exercise you will build a small RBAC setup from scratch, then see how ArgoCD uses the same mechanism and what breaks when those permissions are removed.

## Part 1: Create your own RBAC setup

Create the following three resources and apply them to your cluster:

1. A **ServiceAccount** named `pod-reader` in the `default` namespace.

2. A **Role** named `pod-read-role` in the `default` namespace that grants the following permissions:
   - API group: `""` (core)
   - Resources: `pods`
   - Verbs: `get`, `list`

3. A **RoleBinding** named `pod-reader-binding` that binds the Role to the ServiceAccount.

### Verification

Check what the ServiceAccount is allowed to do:

```
kubectl auth can-i list pods --as=system:serviceaccount:default:pod-reader
kubectl auth can-i delete pods --as=system:serviceaccount:default:pod-reader
```

The first command should return `yes`, the second `no`.

## Part 2: Look at ArgoCD's ServiceAccount

ArgoCD also runs under a ServiceAccount and needs broad permissions to deploy resources into your cluster. Run:

```
kubectl get serviceaccount -n argocd
kubectl describe clusterrolebinding argocd-application-controller
```

You should see that the `argocd-application-controller` ServiceAccount is bound to a ClusterRole that allows it to manage resources across the whole cluster.

## Part 3: Break it

Delete the ClusterRoleBinding that gives ArgoCD its permissions:

```
kubectl delete clusterrolebinding argocd-application-controller
```

Now go to the ArgoCD UI and trigger a manual sync on your application. Check the sync status and the logs - ArgoCD should report a permissions error and fail to deploy.

> **Tip:** You can also check `kubectl get events -n argocd` for error messages.

## Part 4: Restore it

Re-apply the original ArgoCD install manifest to restore all missing resources:

```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Wait a moment, then trigger another sync in the ArgoCD UI. The sync should succeed again.


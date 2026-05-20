# Exercise 13: Horizontal Pod Autoscaler (HPA)

The app is seeing unpredictable traffic spikes. Instead of manually adjusting the replica count every time, we want Kubernetes to scale it automatically.

## Preparation

HPA relies on **metrics-server** to read CPU usage from your Pods. Since we are using minikube, enable it first:

```
minikube addons enable metrics-server
```

Wait about a minute, then verify it is running:

```
kubectl get pods -n kube-system | grep metrics-server
```

> **Important:** HPA requires that your containers have **resource requests** defined. Without them, the autoscaler has no baseline to calculate a percentage from. You will need to add a `resources` section to your Deployment's container spec.

## Task

1. **Update your Deployment** from exercise 6 to include CPU resource requests and limits.

   First, check what the app is currently consuming:

   ```
   kubectl top pods
   ```

   Use the observed CPU value as the basis for your `requests`. Set your `limits` to roughly 2-3x that value.

   > **Tip:** `kubectl top pods` shows CPU in millicores (`m`). Make sure your YAML uses the same units.

2. **Create an HPA** that automatically scales the `training-app` Deployment:
   - Minimum replicas: `2`
   - Maximum replicas: `10`
   - Target CPU utilization: `30%`

Apply both files to your cluster.

## Verification

Run the following commands to check that your HPA is active:

- `kubectl get hpa` - check that the HPA is listed with current and target CPU%
- `kubectl describe hpa <hpa_name>` - look at the *Conditions* and *Events* sections

> **Note:** It may take a minute or two before the HPA shows actual CPU metrics instead of `<unknown>`. metrics-server needs time to collect data.

## Bonus: Trigger a Scale-Out

To see the HPA in action, run a load generator against your app's Service (replace `<cluster-ip>` with your Service's ClusterIP):

```
kubectl run load-generator --image=busybox --restart=Never -- /bin/sh -c "while true; do wget -q -O- http://<cluster-ip>; done"
```

Watch the HPA scale in real time:

```
kubectl get hpa -w
```

When done, delete the load generator:

```
kubectl delete pod load-generator
```

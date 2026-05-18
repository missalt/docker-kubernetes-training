# Exercise 4: Building your first Pod

Kubernetes Time! We now want to deploy our previously built image to Kubernetes.

## Creating a local Kubernetes Cluster

In an Enterprise-Level environment, you typically would set up a Kubernetes Cluster on a dedicated Server (or let your IT department do it for you).
For testing something locally, this is typically not required. Thus, for this and the following exercises, we will create a local Kubernetes Cluster using *minikube*. Since the Kubernetes-API is standardised, any type of Kubernetes objects we are creating in our minikube-Cluster can later be used in any other cluster.

Now, create your cluster by running *minikube start*
After some time, your cluster will have been created and you can now access it using the Kubernetes CLI, kubectl.

Try *kubectl get pods -A* to verify everything is working. You should see some kube-system Pods, which should include some Kubernetes Components which are now known to you.


## The Task 

Your next task is relatively simple! Open DockerHub and search for the image "missalt/training". Go on Tags and look for the latest one. Create a pod.yaml-File which defines a container using this image. Then, *apply* that Pod-definition to your Kubernetes Cluster and see what happens: 

*kubectl get pods* should now show your newly created Pod in the "Running" state.

To verify that this Pod is running, we need to create some kind of networking route inside the cluster which allows us to access our containers from outside it. 
The simplest way to achieve this is to create a *port-forwarding* using kubectl, which maps a localhost port to some Pod in the Cluster: 

*kubectl port-forward pod/<pod_name> <localhost_port>:<container_port>*

This command, if configured correctly, should make it possible for you to access your Pod via localhost. Note that this is not possible on Cloud-Based-Clusters!

## Notes regarding this task

You may think that using *imperative* commands like port-forward goes against the core principles and Kubernetes and is also not a solution to more complex problems, like routing based on HTTP Hosts (e.g. amazon.com/cart goes to Pod 1, amazon.com/payment goes to Pod 2). If you do, you're absolutely right! Port-forward is great for testing simple applications, but accessing apps from outside the Cluster is typically done using Ingress. We will talk about this at the end of the Kubernetes Networking section.

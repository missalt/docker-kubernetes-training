# Exercise 9: PVC

We now want to start recreating our compose environment from Exercise 3. We already have the first part: our application is up and running.
Now, we want to create a Persistent Volume Claim and a Deployment for redis.

## Task 

1. Create a PVC which requests 1GB of Storage. Set the AccessMode in such a way that only 1 Pod can access it at a time. Use dynamic provisioning (in minikube, this is done by not including any storageClass).
2. Fill in the blanks in the given Deployment-File.
3. Deploy everything to our Cluster.


## Verification

First, make sure everything worked so far: *kubectl get pvc* SHOULD now show your PVC in the Status "Bound". If it does, congrats! You're good to go.

You will notice that our application has not changed so far. This has 2 reasons: 
1. Our App only checks for Redis to connect to on *restart*.
2. As of now, we have not configured our App with the REDIS_URL Environment-variable.

We are going to do the latter using a ConfigMap in the next exercise.
# Health Checks

If your app malfunctions, the pod and container can still be running but the app will no longer be working

We can detect and resolve problems with our application using health checks.

There are 2 types of health checks we can run:
  - running a command in the container periodically
  - or more commonly, we can run checks on a URL (HTTP)

For apps in production, it is highly recommended to **always** have health checks in place to ensure availability and resiliency of the apps

This is true even for apps behind a load balancer on AWS etc, as the load balancer health checks are not checking the health of the application, only if the nodeport of the container is accessible

**Example health check:**

Under the `container:` section of the definition file set the following:

```
  livenessProbe:
    httpGet:
      path: /
      port: 3000
    initialDelaySeconds: 15
    timeoutSeconds: 30
```

Now create the deployment [helloworld-deployment.yml](definitions/helloworld-deployment.yml)
  - `kubectl create -f definitions/helloworld-deployment.yml`

We can then see which health checks have been applied to our pods:
  - `kubectl get pods` to get the name of the pods
  - `kubectl describe pod <pod_name>` then look for the `Liveness:` line

With the above example deployment definition we are running 3 replicas of our pod. If one of these develops an issue and the health check no longer gets a successful HTTP code back from the application, then Kubernetes will automatically delete the pod, and start a new pod in its place.

This ensures that if one pod errors, end users will no longer be routed to the pod with the error state, and instead will be routed to healthy pods, while kubernetes in the meantime brings up a new pod to replace the unhealthy pod.

We can edit an already running deployment to add a health check if we wish:
  - `kubectl edit deployment/<deployment_name>` and add the above `livenessProbe:` example to the `container:` section

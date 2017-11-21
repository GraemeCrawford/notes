## Replication Sets

Next generation replica controller

supports a new selector, can do selection based on filtering according to a set of values
  - for example
    - environment: dev or qa

Whereas the replication controller could only for example do environment == dev

This replica set rather than the replication controller is used by the deployment object.


## Deployments

A declaration in Kubernetes that allows you to do app deployments and updates

You define the state of your application and Kubernetes will then make sure the cluster matches your desired state

Using replication sets or replication controllers is cumbersome to deploy apps - too much manual work for updates and rollbacks etc

The **Deployment Object** is easier to use and gives you more possibilities

  - you can create/update a Deployment - (deploy and update your app)
  - do rolling updates - (which introduces the prospect of zero downtime Deployments)
    - deployed in steps
  - roll back Deployments
  - pause/resume - roll out to only a certain percentage of your running pods


## Useful Commands

|Command|Description|
|---|---|
|`kubectl get deployments`| get info on current deployments|
|`kubectl get rs` |get info on replica sets|
|`kubectl get pods --show-labels` |get pods and show the labels attached to those pods|
|`kubectl rollout status` |get deployment status|
|`kubectl set image deployment/helloworld-deployment k8s-demo=k8s-demo:2` |run k8s-demo with the image label version 2|
|`kubectl edit deployment/helloworld-deployment` |edit the deployment oject|
|`kubectl rollout status deployment/helloworld-deployment` |get the status of the rollout|
|`kubectl rollout history deployment/helloworld-deployment` |get the rollout history|
|`kubectl rollout undo deployment/helloworld-deployment` |roll back to previous version|
|`kubectl rollout undo deployment/helloworld-deployment --to-revision=n` |roll back to any version|

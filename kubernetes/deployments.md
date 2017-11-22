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

# Doing the deployment

## Create Deployment file

Create helloworld-deployment.yml with the following contents [helloworld-deployment.yml](./definitions/helloworld-deployment.yml)

  - `kubectl create -f helloworld-deployment.yml`

Check the status of the deployment:
  - `kubectl get deployments`
  - `kubectl rollout status deployment/<deployment_name>`


*Now if we like we can change the image being used by the deployment by editing the deployment:*

  - `kubectl set image deployment/helloworld-deployment <container_name>=<image_name:tag>`

  *Note: the container_name is the name you gave the container in the deployment definition container spec section*

  **This is how we'd manually update the version of the running container image**

## Undo/Rollback Deployments

To undo the most recent deployment and revert back to the previous deployment, we'd run the following:

  - `kubectl rollout undo deployment/helloworld-deployment`

If we want to rollback to a specific deployment we would run the following:

*First we want to get the rollout history*
  - `kubectl rollout history deployment/helloworld-deployment`

*Then we want to specify which revision we'd like to revert to:*
  - `kubectl rollout undo deployment/<deployment_name> --to-revision=1`

**By default, Kubernetes only gives us the last 2 deployments. We can force Kubernetes to keep a longer history of deployments by adding the following line to the `helloworld-deployment.yml` definition file under the app spec section:**

  - `revisionHistoryLimit: 100`

## Scaling Pods

If app is stateless it can be horizontally scaled.

  - app doesn't have a state
    - doesn't write local files
    - doesn't contain any session data

If both don't have their own state - request to one pod will lead to the same result as the other, then the pod is probably stateless.

All traditional databases are stateful and can't be split over multiple instances

Most web apps can be made stateless:
  - session management must be done outside the container
  - can't store data in the container
    - store it in memcache/redis/any other database outside the container

Any files that need to be saved, cannot be saved locally in the container
  - files will be lost when the container is stopped
    - store in S3 on AWS etc


## Replication controller

- The replication controller will ensure a specified number of pod replicas will run at all times

- Pods created by the replication controller will be auto replaced if they fail, get deleted or are terminated

- Recommended if you want to ensure 1 pod is always running even after reboots
  - you can run a replication controller with just 1 replica
    - makes sure that the pod is always running even if the node crashes


## Example

Create the **replica controller** yaml file for the **helloworld** application

  - [helloworld-rep-controller.yml](./definitions/helloworld-rep-controller.yml)

**Create the replica controller from the file definition**
  - `kubectl create -f helloworld-rep-controller.yml`

**Check that the pods are being created**
  - `kubectl get pods`
    - This should show the number of pods being created, corresponding to the number of replicas set in the replica-controller definition file

**You can check the description of the pods as follows**
  - `kubectl desc helloworld-controller-t5rk8`

**Put the replica controller through its paces by deleting one of your pods**
  - `kubectl delete pod helloworld-controller-t5rk8`

**Check that the replica controller instantly creates a new pod to keep the number of replicas you have requested**
  - `kubectl get pods`
    - *This should show the pod you just deleted being terminated, and a new pod created in it's place*

**You can now manually specify to the replica controller how many replicas you want**
  - By using the file:
    - `kubectl scale --replicas=4 -f helloworld-rep-controller.yml`


  - By using the replica controllers name
    - `kubectl get rc`
    - `kubectl scale --replicas=1 rc/<replica_controller_name>`

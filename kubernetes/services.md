## Services

Pods are dynamic, they come and go on the cluster.
  - when using a **replication controller**, pods are terminated and created during scaling operations

  - When using **deployments**, when updating the image version, pods are terminated and new pods take their place

This is why pods should **never** be accessed directly, but instead via a **Service**
  - A **service** is the **logical bridge** between the "mortal" pods and other services or end-users

Creating a service will create an endpoint for your pod(s):
  - a ClusterIP: only reachable from within the cluster (default)
  - a NodePort: a port that is the same on each node that is also reachable externally
  - a LoadBalancer: created by the cloud provider where your cluster is running, that will route external traffic to every node on the NodePort

*These options only allow you to create virtual IP's or ports*

#### You can also use DNS names

  - ExternalName can provide a DNS name for the service
  - e.g. for service discovery using DNS
  - only works when the DNS add-on is enabled

---
### Services
This is an example service definition file: [Example Service Definition](definitions/service-example.yml)

By default services can only run on ports between **30000** and **32767** (**this can be changed by adding `--service-node-port-range=X-Y` to the kube-apiserver in the init scripts**)

#### Create the service:
  - `kubectl create -f definitions/nodeport-service-example.yml`

#### Check the details of the service:
  - `kubectl get svc helloworld-service`

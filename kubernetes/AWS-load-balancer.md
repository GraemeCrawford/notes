## Accessing your Applications via an AWS Load Balancer

In a real world scenario you need to be able to access your app from outside the cluster

  - On AWS you can do this by adding an External Load Balancer
  - The load balancer will route traffic to the correct Kubernetes pod
  - There are other solutions for cloud providers who don't offer Load Balancers
    - Such as running your own haproxy / nginx load balancer in front of your cluster
    - Or expose ports directly

#### Create a service definition file to match your previously created Pod

**Previously created Pod file**

```
apiVersion: v1
kind: Pod
metadata:
  name: helloworld.graemetech.com
  labels:
    app: helloworld
spec:
  containers:
  - name: k8s-demo
    image: wardviaene/k8s-demo
    ports:
    - name: nodejs-port
      containerPort: 3000
```

**Matching Load Balancer Service Definition File**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: helloworld-service
spec:
  ports:
  - port: 80
    targetPort: nodejs-port
    protocol: TCP
  selector:
    app: helloworld
  type: LoadBalancer
```

Create the Pod if it hasn't already been created:

- `kubectl create -f path/to/pod-file`

Now create the Load Balancer service

- `kubectl create -f path/to/lb-file`

# Check that the load balancer has been created on AWS

Login to AWS > Services > EC2 > Load Balancers

Your load balancer should be listed



# Create AWS Route53 route for accessing your app from the internet via your domain name


## Accessing your Applications via an AWS Load Balancer

In a real world scenario you need to be able to access your app from outside the cluster

  - On AWS you can do this by adding an External Load Balancer
  - The load balancer will route traffic to the correct Kubernetes pod
  - There are other solutions for cloud providers who don't offer Load Balancers
    - Such as running your own haproxy / nginx load balancer in front of your cluster
    - Or expose ports directly

#### Create a service definition file

```yaml
apiVersion: v1
kind: service
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

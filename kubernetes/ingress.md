Allows inbound connections to the cluster

an alternative to external loadbalancer and nodeports

allows you to easily expose services that need to be accessible from the outside to the cluster

you can run your own ingress controller (basically a loadbalancer) within the kubernetes cluster

There are default ingress controllers available, or you can write your own ingress controller

Created using the ingress object:

An example of using multiple hosts:

```
apiVersion: extensions/v1/beta1
kind: Ingress
metadata:
  name: helloworld-rules
spec:
  rules:
  - host: helloworld-v1-example.com
  http:
    paths:
    - path: /
      backend:
        serviceName: helloworld-v1
        servicePort: 80
  - host: helloworld-v2.example.com
    http:
      paths:
      - path: /
        backend:
          serviceName: helloworld-v2
          servicePort:80
```

An example of using multiple paths:

```
apiVersion: extensions/v1/beta1
kind: Ingress
metadata:
  name: helloworld-rules
spec:
  rules:
  - host: helloworld-v1-example.com
  http:
    paths:
    - path: /
      backend:
        serviceName: helloworld-v1
        servicePort: 80
    - path: /app
      frontend:
        serviceName: helloworld-frontend
        servicePort: 80
```

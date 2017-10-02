## Running Applications on Kubernetes cluster

** Now we can run applications on the Kubernetes cluster**

- ```kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080```

To check if the deployment is running:

- ```kubectl get deployment```
```
NAME             DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-minikube   1         1         1            1           3m
```

If you want to get rid of the deployment you can delete it with:

- ```kubectl delete deployment hello-minikube```

Then expose the port we asked it to run on to the internet:

- ```kubectl expose deployment hello-minikube --type=NodePort```

Now we can find out which node it's running on, the IP and port that the application is running on etc:

- ```kubectl get service```

  ```
  NAME             CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
  hello-minikube   100.x.x.15      <nodes>       8080:30287/TCP   3m
  kubernetes       100.x.x.1       <none>        443/TCP          59m
  ```
**Important** The port we are interested in here is the external port which is the one after the colon **:** **30287** in this case.

  We will need to open this port on the AWS firewall.

  **Go to your AWS account, Networking & Content Delivery > VPC > Security Groups**

  You want to find the **masters**.dns.name group and select it and go to **Inbound Rules** > Edit > Add another rule:

  ```Custom TCP Rule - TCP (6) - <port from above> - 0.0.0.0/0 (or specific IP range) - Description you want```

**Now to access the app from a web browser you will need to get the public IP for the kubernetes api:**

- Go to your AWS account > Route53 > Hosted Zones > your kubernetes domain
- Look for the entry for api.yourdomain.com (A record) and take the IP address from here.
- You will now be able to hit the IP:Port from your browser ```52.123.x.x:30287``` **or** dnsname.port ```api.yourdomain.com:30287```

## Pods

- A pod describes an application running on Kubernetes

- A pod can contain one or more tightly coupled containers, that make up the application
  - Those apps can communicate with each other using their local port numbers


- **Containers in the same pod can communicate with each other as if they are the same host.** For example you have a front end app running on port 80, and a back end app running on port 5000 both in the same pod. If you open a shell into the the front end container, you can access **localhost:5000** and you will hit the back end app.

**Example Pod definition file:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: helloworld.example.com
  labels:
    app: helloworld
spec:
  containers:
  - name: k8s-demo
    image: wardviaene/k8s-demo
    ports:
    - containerPort: 3000
```

## Create a Pod

After you have created your pod definition file above, run the following command to create the pod containing your app:

- ```kubectl create -f path_to/<name of definition file>```

Check the pod has been created successfully:

- ```kubectl get pods```

  ```
  NAME                     READY     STATUS              RESTARTS   AGE
  helloworld.example.com   1/1       Running   0          2m
  ```

## Useful Commands:
| Command | Description |
|---------|-------------|
|**kubectl get pods** | Get info about all running pods |
|**kubectl describe pods pod_name** | Describe one pod |
|**kubectl expose pod pod_name --port=444 --name=frontend** | Expose the port of a pod (creates new service) |
|**kubectl port-forward pod_name 8080** | Port forward the exposed pod port to your local machine |
|**kubectl attach pod_name -i** | Attach to the pod |
|**kubectl exec pod_name -- command** | Execute a command on the pod |
|**kubectl label pods pod_name mylabel=awesome** | Add a new label to a pod |
|**kubectl run -i --tty busybox --image=busybox --restart=Never -- sh** | Run a shell in a pod - very useful for debugging |
|**kubectl describe svc service_name** | Describe the running service |



**Port forward the Pod port to your local machine to enable you to browse to it locally via web browser:**

- ```kubectl port-forward helloworld.example.com 8080:3000```

  Now hit port **8080** on your local machine and you will be forwarded to your app in the pod on the cluster

**Expose your app for external access:**

```kubectl expose pod helloworld.example.com --type=NodePort --name helloworld-service```

**Check which port the app has been exposed on:**

- ```kubectl get svc```
  ```
  NAME                 CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
  helloworld-service   100.xx.xx.177   <nodes>       3000:30815/TCP   25m
  kubernetes           100.x.x.1       <none>        443/TCP          3h
  ```

On this occasion, port **30815** is the external port.

  - Open this port on your AWS masters security group
  - You should now be able to browse to <master_IP>:30815















#

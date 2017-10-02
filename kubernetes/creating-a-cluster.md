# Creating a cluster

In order to stop kops asking you to specify the S3 bucket everytime you run a command, add the following to your .bashrc or .zshrc

- ```vim ~/.zshrc```
  - ```export export KOPS_STATE_STORE=s3://kops-state-b33445566``` **replace with the name of your kops-state bucket**


**For the below instructions, substitute dns.name.com with your fully qualified domain name**

**Check what will be created with**

```kops create cluster --name=dns.name.com --state=s3://kops-state-b33445566 --zones=eu-west-1 --node-count=2 --node-size=t2.micro --master-size=t2.micro --dns-zone=dns.name.com```

**Create the cluster from the above command with**

```kops update cluster dns.name.com --yes```

  - **You could also edit the cluster first if you wish:**

  ```kops edit cluster dns.name.com```

  - **Edit the node instance group:**

  ``` kops edit ig --name=dns.name.com nodes```

  - **Edit the master instance group:**

  ``` kops edit ig --name=dns.name.com master-eu-west-1a```  (substitute with your masters name)


**Check the running clusters**

- ```kops get cluster```

**Validate your cluster:**

- ``` kops validate cluster ```

The cluster will take a few minutes to get to a stable point in order for this command to complete successfully - if you get the following error give the creation some more time to complete:

- ```cannot get nodes for "domain.com": Get https://api.domain.com/api/v1/nodes: dial tcp 203.x.y.z:443: getsockopt: network is unreachable```


**Successfully provisioned cluster message:**

```
Using cluster from kubectl context: domain.com

Validating cluster domain.com

INSTANCE GROUPS
NAME			         ROLE	  MACHINETYPE	 MIN	  MAX	  SUBNETS
master-eu-west-1a	        Master	  t2.micro	  1	    1	    eu-west-1a
nodes			        Node	  t2.micro	    2	    2	    eu-west-1a

NODE STATUS
NAME			   ROLE	    READY
ip-172-x-y-z.eu	    node	    True
ip-172-x-y-z.eu	    master	  True
ip-172-x-y-z.eu	    node	    True

Your cluster domain.com is ready
```



**Delete your running cluster**

- ```kops delete cluster dns.name.com --yes```


**The cluster creation command creates the following local file which contains certificates and passwords for connecting to your cluster**

- ```~/.kube/config```
  - **You should keep this file private!!**

**You can now check the status of the nodes you have just created:**

- ```kubectl get node```

  ```
  NAME                                        STATUS         AGE       VERSION
  ip-172-x-x-255.eu-west-1.compute.internal   Ready,node     41m       v1.7.2
  ip-172-x-x-70.eu-west-1.compute.internal    Ready,master   43m       v1.7.2
  ip-172-x-x-64.eu-west-1.compute.internal    Ready,node     41m       v1.7.2
  ```






###

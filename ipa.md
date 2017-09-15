### Installing the Server

```yum install ipa-server```

#### *I didn't do this as I am using external DNS, but:*

To install the packages required for a server with integrated DNS services:#

``` yum install ipa-server ipa-server-dns ```


- ```ipa-server-install```

- ```kinit admin```



```
Be sure to back up the CA certificates stored in /root/cacert.p12
These files are required to create replicas.
The password for these files is the Directory Manager password
```

### Manage the IdM domain

To be able to manage the IdM domain from the client machine, install the ```ipa-admintools``` package as well. The package installs command-line tools for IdM administrators. If you want to use the client machine as a regular client system, ```ipa-admintools``` is not necessary.

- ``` yum install -y ipa-admintools```

### Installing a new client:

- ``` ipa-client-install ```


### 3.6. TESTING THE NEW CLIENT

Check that the client can obtain information about users defined on the server. For example, to check the default admin user use the ``` id admin ``` command:

```
[user@client ~]$ id admin
uid=1234560000(admin) gid=1234560000(admins) groups=1234560000(admins)
```

### Uninstalling a client

``` ipa-client-install --uninstall ```


### Web UI

Instructions can be found here:

[WebUI](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Linux_Domain_Identity_Authentication_and_Policy_Guide/using-the-ui.html)


### Building a Replica

The replica(s) of a master server must be the same version of the FreeIPA server software or **later**. The replica mase be running on the same version of the OS or **later**.

[Building a replica](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Linux_Domain_Identity_Authentication_and_Policy_Guide/replica-required-packages.html)

Follow the same instructions as building your first server:

[Building an IdM Server](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Linux_Domain_Identity_Authentication_and_Policy_Guide/required-packages.html)


###### Steps:

- ```yum install ipa-server```

*To install the packages required for a server with integrated DNS services*

- ```yum install ipa-server ipa-server-dns```

##### You can either make an existing client a replica:

- ```kinit admin```
- ```ipa-replica-install --principal admin --admin-password admin_password```

##### Or install a replica on a machine that's not already a client:

- ```ipa-replica-install --principal admin --admin-password admin_password```


**Update**

I had to end up installing a client on the machine first, and then do the ```ipa-replica-install``` command afterwords. **Make sure firewall rules are set up correctly, or ```iptables -F``` to flush the firewall rules on a test machine for ease otherwise when setting up the replica the connection will faill and the task will fail.

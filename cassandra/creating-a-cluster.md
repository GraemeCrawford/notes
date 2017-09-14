### Creating a Cassandra cluster

To create a Cassandra cluster, first you must install Cassandra onto each node in the cluster. Follow the installation instructions here:

- [Install Cassandra](installation.md)

#### After installing Cassandra:

**Step 1:**

*Delete the default data:*

- ``` sudo systemctl stop cassandra```
- ```sudo rm -rf /var/lib/cassandra/data/system/*```

**Step 2:**

*Configue the Cluster:*

Edit the following file:
- ```vim /etc/cassandra/cassandra.yaml```

  - **Change the following settings:**
   - ```cluster_name: 'cassandracluster1```

   - ```- seeds: "IP-SERVER1,IP-SERVER2..."```

   - ```listen_address: 192.168.x.x ```
     - This is IP address that other nodes in the cluster will use to connect to this one. It defaults to localhost and needs changed to the IP address of the node.

   - ```rpc_address: 127.0.0.1```
     -  This is the IP address for remote procedure calls. It defaults to localhost. If the server's hostname is properly configured, leave this as is. Otherwise, change to server's IP address or the loopback address (127.0.0.1).

   - ```endpoint_snitch: GossipingPropertyFileSnitch```
      - Name of the snitch, which is what tells Cassandra about what its network looks like. This defaults to SimpleSnitch, which is used for networks in one datacenter. In our case, we'll change it to GossipingPropertyFileSnitch, which is preferred for production setups.

   - ```auto_bootstrap: False ``` -- add this into the file

     - This directive is not in the configuration file, so it has to be added and set to false. This makes new nodes automatically use the right data. It is optional if you're adding nodes to an existing cluster, but required when you're initializing a fresh cluster, that is, one with no data.

**Step 3**

*Configure the Firewall*

At this point, the cluster has been configured, but the nodes are not communicating. In this step, we'll configure the firewall to allow Cassandra traffic.

First restart Cassandra:

- ```systemctl restart cassandra```

Allow Cassandra some time to start up and then check the status:

- ```sudo nodetool status```

We need to open the following ports to allow the machines in the cluster to communicate:

- 7000 - which is the TCP port for commands and data.
- 9042 - which is the TCP port for the native transport server.
  - **cqlsh**, the Cassandra command line utility, will connect to the cluster through this port.

Edit the **/etc/iptables/rules.v4** file:

- ```sudo vim /etc/iptables/rules.v4```

**For the next step, you must add the line for each server IP in the cluster to allow traffic from the specific IP through.**

**Example**: Add the following line to **server-1**:

- ```-A INPUT -p tcp -s server-2-IP -m multiport --dports 7000,9042 -m state --state NEW,ESTABLISHED -j ACCEPT ```

Note the IP address of **server2** will be inserted here.

On **server-2** we will add the IP address of **server-1** to allow *server-1** traffic

- ```-A INPUT -p tcp -s server-1-IP -m multiport --dports 7000,9042 -m state --state NEW,ESTABLISHED -j ACCEPT ```

Now restart iptables-persistent and then save the rules, which on Debian 9 which I am currently using is **netfilter-persistent**

Commands:

- ```sudo netfilter-persistent restart ```
- ```sudo netfilter-persistent save```
- ```sudo netfilter-persistent reload```



**Step 4**

Check the Cluster Status:

- ```sudo nodetool status```

```
root@cassandra-1:/# nodetool status
Datacenter: dc1
===============
Status=Up/Down
|/ State=Normal/Leaving/Joining/Moving
--  Address         Load       Tokens       Owns (effective)  Host ID                               Rack
UN  192.168.x.x   137.21 KiB   256          100.0%            xxxx-xxxx-xxxx-xxxx-xxxx              rack1
UN  192.168.x.y   208.47 KiB   256          100.0%            xxxx-xxxx-xxxx-xxxx-xxxx              rack1

```


##### You should now be able to see all of the nodes you configured and you have just successfully set up a multi-node Cassandra cluster.










  ---

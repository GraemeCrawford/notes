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

   - ```auto_bootstrap: ```

     - This directive is not in the configuration file, so it has to be added and set to false. This makes new nodes automatically use the right data. It is optional if you're adding nodes to an existing cluster, but required when you're initializing a fresh cluster, that is, one with no data.

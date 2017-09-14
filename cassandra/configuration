## Configuring Cassandra
For running Cassandra on a single node, the steps above are enough, you don’t really need to change any configuration. However, when you deploy a cluster of nodes, or use clients that are not on the same host, then there are some parameters that must be changed.

The Cassandra configuration files can be found in the conf directory of tarballs.

For packages, the configuration files will be located in **/etc/cassandra**

### Main runtime properties

Most of configuration in Cassandra is done via yaml properties that can be set in **cassandra.yaml**. At a minimum you should consider setting the following properties:

- **cluster_name**: the name of your cluster.

- **seeds**: a comma separated list of the IP addresses of your cluster seeds.

- **storage_port**: you don’t necessarily need to change this but make sure that there are no firewalls blocking this port.

- **listen_address**: the IP address of your node, this is what allows other nodes to communicate with this node so it is important that you change it. Alternatively, you can set listen_interface to tell Cassandra which interface to use, and consecutively which address to use. Set only one, not both.

- **native_transport_port**: as for storage_port, make sure this port is not blocked by firewalls as clients will communicate with Cassandra on this port.

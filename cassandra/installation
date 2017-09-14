## Installation of Apache Cassandra

### Debian

##### Installation from Debian packages
Add the Apache repository of Cassandra to /etc/apt/sources.list.d/cassandra.sources.list, for example for version 3.6:

``` echo "deb http://www.apache.org/dist/cassandra/debian 36x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list```

Add the Apache Cassandra repository keys:

- *Install curl* ```sudo apt-get install curl```

```curl https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -```

Update the repositories:

```sudo apt-get update```

**If you encounter this error:**

*GPG error: http://www.apache.org 36x InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY A278B781FE4B2BDA*

**Then add the public key A278B781FE4B2BDA as follows:**

```sudo apt-key adv --keyserver pool.sks-keyservers.net --recv-key A278B781FE4B2BDA```

and repeat ```sudo apt-get update```. The actual key may be different, you get it from the error message itself. For a full list of Apache contributors public keys, you can refer to this link.

Install Cassandra:

``` sudo apt-get install cassandra```

You can control Cassandra with:

- ```sudo service cassandra start```
- ```sudo service cassandra stop```

However, normally the service will start automatically. For this reason be sure to stop it if you need to make any configuration changes.

Verify that Cassandra is running by invoking:

```nodetool status```



The default location of configuration files is:

- **/etc/cassandra**

The default location of log and data directories are:

- **/var/log/cassandra/**
- **/var/lib/cassandra**

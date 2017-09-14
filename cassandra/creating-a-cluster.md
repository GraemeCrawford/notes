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

   - ```rpc_address: 192.168.x.x```  
     - **Make sure to change this to the servers IP otherwise you won't be able to connect remotely...**
       This is the IP address for remote procedure calls. It defaults to localhost. If the server's hostname is properly configured, leave this as is. Otherwise, change to server's IP address or the loopback address (127.0.0.1).

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

## Example cassandra.yaml config file:

```yaml
cluster_name: 'Test Cluster'

num_tokens: 256

hinted_handoff_enabled: true
max_hint_window_in_ms: 10800000 # 3 hours

hinted_handoff_throttle_in_kb: 1024

max_hints_delivery_threads: 2

hints_flush_period_in_ms: 10000

max_hints_file_size_in_mb: 128

batchlog_replay_throttle_in_kb: 1024

authenticator: AllowAllAuthenticator

authorizer: AllowAllAuthorizer

role_manager: CassandraRoleManager

roles_validity_in_ms: 2000

permissions_validity_in_ms: 2000

credentials_validity_in_ms: 2000

partitioner: org.apache.cassandra.dht.Murmur3Partitioner

data_file_directories:
    - /var/lib/cassandra/data

commitlog_directory: /var/lib/cassandra/commitlog

disk_failure_policy: stop

commit_failure_policy: stop

prepared_statements_cache_size_mb:

thrift_prepared_statements_cache_size_mb:

key_cache_size_in_mb:

key_cache_save_period: 14400

row_cache_size_in_mb: 0

row_cache_save_period: 0

counter_cache_size_in_mb:

counter_cache_save_period: 7200

saved_caches_directory: /var/lib/cassandra/saved_caches

commitlog_sync: periodic
commitlog_sync_period_in_ms: 10000

commitlog_segment_size_in_mb: 32


seed_provider:
    - class_name: org.apache.cassandra.locator.SimpleSeedProvider
      parameters:
          - seeds: "192.168.x.y,192.168.x.x"

concurrent_reads: 32
concurrent_writes: 32
concurrent_counter_writes: 32

concurrent_materialized_view_writes: 32

memtable_allocation_type: heap_buffers

index_summary_capacity_in_mb:

index_summary_resize_interval_in_minutes: 60

trickle_fsync: false
trickle_fsync_interval_in_kb: 10240

storage_port: 7000

ssl_storage_port: 7001

listen_address: 192.168.x.x

start_native_transport: true
native_transport_port: 9042

start_rpc: true

rpc_address: 192.168.x.x

rpc_port: 9160

rpc_keepalive: true

rpc_server_type: sync

thrift_framed_transport_size_in_mb: 15

incremental_backups: false

snapshot_before_compaction: false

auto_snapshot: true

column_index_size_in_kb: 64
column_index_cache_size_in_kb: 2

compaction_throughput_mb_per_sec: 16

sstable_preemptive_open_interval_in_mb: 50

read_request_timeout_in_ms: 5000
range_request_timeout_in_ms: 10000
write_request_timeout_in_ms: 2000
counter_write_request_timeout_in_ms: 5000
cas_contention_timeout_in_ms: 1000
truncate_request_timeout_in_ms: 60000
request_timeout_in_ms: 10000

cross_node_timeout: false

endpoint_snitch: GossipingPropertyFileSnitch

dynamic_snitch_update_interval_in_ms: 100
dynamic_snitch_reset_interval_in_ms: 600000
dynamic_snitch_badness_threshold: 0.1

request_scheduler: org.apache.cassandra.scheduler.NoScheduler

server_encryption_options:
    internode_encryption: none
    keystore: conf/.keystore
    keystore_password: cassandra
    truststore: conf/.truststore
    truststore_password: cassandra

client_encryption_options:
    enabled: false
    optional: false
    keystore: conf/.keystore
    keystore_password: cassandra

internode_compression: dc

inter_dc_tcp_nodelay: false

tracetype_query_ttl: 86400
tracetype_repair_ttl: 604800

enable_user_defined_functions: false

enable_scripted_user_defined_functions: false

windows_timer_interval: 1

transparent_data_encryption_options:
    enabled: false
    chunk_length_kb: 64
    cipher: AES/CBC/PKCS5Padding
    key_alias: testing:1
    key_provider:
      - class_name: org.apache.cassandra.security.JKSKeyProvider
        parameters:
          - keystore: conf/.keystore
            keystore_password: cassandra
            store_type: JCEKS
            key_password: cassandra

tombstone_warn_threshold: 1000
tombstone_failure_threshold: 100000

batch_size_warn_threshold_in_kb: 5

batch_size_fail_threshold_in_kb: 50

unlogged_batch_across_partitions_warn_threshold: 10

compaction_large_partition_warning_threshold_mb: 100

gc_warn_threshold_in_ms: 1000

auto_bootstrap: false
```







  ---

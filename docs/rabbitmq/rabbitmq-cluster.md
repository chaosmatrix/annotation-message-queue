# Note

### Things shoud be consider
1. Program
    1. client should auto reconnect while cluster node failed
        * How to avoid this situation
            1. Server side support load balance with health check
                * tcp load balancer for all cluster nodes (hash algorithm)
            2. Client side
                * dns server with health check and auto update record, domain name has very short ttl (ugly)
    2. if queue not mirror queu, while reconnect to another node, must re-create queue

2. Cluster Configuration
    1. RAM or Disk Node
        * RAM nodes do not provide higher message rates, only improve resource
          manage oprations, aka. adding/removing vhosts/exchanges/queues
        * RAM nodes are a special case that can be used to improve the
          performance clusters with high queue, exchange, or binding churn
        * At least one Disk node in cluster, so can't remove last Disk node in cluster
        * RAM nodes store internal database tables in RAM, not message
    2. Restart while crash/shutdown
        * Node must be restart in sequence of they shutdown/crash, or it it
          will wait the other node become online in 30s
        * use `rabbitmqctl forget_cluster_node` to remove node, if the node
          can't be restart after crash/shutdown
        * use `rabbitmqctl force_boot` if don't know which nodes's
          crash/shutdown sequence, or all node think all the other nodes stoped
          after it
    3. Cluster Nodes Sync Policy
        * vhost/queue must set obviously
        * Sync Policy while new nodes join/rejoin
    4. Meaning Of A Success Operation
        * direct connect node(master) update message info
        * all/part nodes update success (depend on sync policy)
        * time usage: first node updated -> last node updated

3. Partition
    1. while nodes leave and rejoin without sequence
        * first leave, first rejoin

4. Security
    1. Authority
        * SSL/Plain
        * Username and Password
    2. Firewall
        * Network reachable
    3. .erlang_cookie of cluster nodes
        * cluster nodes communication

5. System Configuration Consider
    1. Limits
        * LimitNOFILE
        * LimitNPROC
    2. Resource Alarm
        * memory size
        * disk free space


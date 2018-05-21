title: "Elasticsearch的一些优化配置"
date: 2018-05-21 15:00:00 +0800
update: 2018-05-21 15:00:00 +0800
author: me
cover: ""
tags:
    - elasticsearch
preview: Elasticsearch的一些优化配置

---

> Elasticsearch在生产环境中，需要做一系列的优化，以达到最佳的性能。虽然网上有些优化的建议和经验，但却不一定有效，下面就结合官网上的介绍和网上的一些指导加上自己在使用过程中的实际处理， 给出更加有说服力的一些意见和建议。
官网的关于生产环境的部署的建议，`https://www.elastic.co/guide/en/elasticsearch/guide/current/deploy.html`

1. 硬件：`https://www.elastic.co/guide/en/elasticsearch/guide/current/hardware.html`
    - 内存：设置ES_HEAP_SIZE 为机器内存的1/2, 但不要超过32G.
    - CPU：官网的意见是， 2~8核足够了， 从我的经验看， 一般是要超过2核的。我的机器用的是32核， top看的话， 也就用了2核多。当然核数多点也无妨的。
    - 磁盘：首选是SSD + RAID0
    - 网络：1 GbE, 10 GbE， 最好不要跨机房，容易造成网络延迟。

2. JVM：`https://www.elastic.co/guide/en/elasticsearch/guide/current/_java_virtual_machine.html`
除了ES_HEAP_SIZE， 不要擅自改变JVM 的参数， 只要使用ES本身的设置就好了。

3. 重要的配置：`https://www.elastic.co/guide/en/elasticsearch/guide/current/important-configuration-changes.html`
ES通常是不需要调优处理的， 如果遇到性能问题， 最好的方法是安排更好的数据布局和增加节点数目。
    1. discovery.zen.minimum_master_nodes:(number of master-eligible nodes / 2) + 1
    防止一个集群里， 出现多个master，造成数据混乱。discovery.zen.minimum_master_nodes的数量应该是有资格成为master的node数量的1/2+1。
    而且随着集群node数量的变化（主要是master-eligible
    node数量的变化）， 需要动态的修改这个参数。可以使用如下的API动态修改。
    ```
    PUT /_cluster/settings
    {
        "persistent" : {
            "discovery.zen.minimum_master_nodes" : 2
        }
    }
    ```
    2. 集群重启的设置：即集群停止工作后重新启动

    为了防止集群重启过程中带来的数据重复备份及平衡转移等不必要的消耗， 可以通过gateway的设置来预防。
        - gateway.recover_after_nodes: x 当有x个node上线之后， 开始集群的recovery 
        - gateway.expected_nodes: y
        - gateway.recover_after_time: 5m 当有y个node加入集群 或者 重启5分钟之后，开始recovery 
    与此相对应的是 重启正在工作的集群中的某个node时的操作，
    disable shard allocation， 防止ES对shards做rebalance
    ```
    PUT/_cluster/settings
    {
    "transient":{
        "cluster.routing.allocation.enable":"none"
    }
    }
    ```
    shutdown single node
    维护并重启该node reenable shard allocation
    ```
    PUT/_cluster/settings
    {
    "transient":{
        "cluster.routing.allocation.enable":"all"
    }
    }
    ```
    平衡之后(status green),再对其它node重复上面的操作。

4. 不需要动的配置：
    1. GC的策略不要修改， 使用默认的就OK了。
    2. threadpool的配置， 使用默认的就好了， 已经是经过验证的最优了。

5. disable SWAP between memory and disk
有3种策略：`https://www.elastic.co/guide/en/elasticsearch/guide/current/heap-sizing.html`，任意其一就行。
我们的服务器，默认使用的vm.swappiness=0，可以通过sudo sysctl -a | grep swap验证。

6. 句柄数和MMap
设置最大句柄数为655360（我们的服务器是默认的），`sudo sysctl -a | grep fs.file-max`得到，可以通过 GET /_nodes/process 验证。
设置MMap: `sysctl -w vm.max_map_count=262144`
或者通过修改`/etc/sysctl.conf`文件中的`vm.max_map_count`永久改变(`sudo sysctl -p /etc/sysctl.conf`生效)。
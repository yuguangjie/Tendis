# Tendisplus

Tendisplus is a high-performance distributed storage system which is fully compatible with the redis protocol. It uses rocksdb as the storage engine, and all data is stored to disk through rocksdb. Users can visit tendisplus using redis client, and the application hardly needs to be changed. However, tendisplus supports storage capacity far exceeding memory, which can greatly reduce user storage costs.

Similar to redis cluster, tendisplus uses a decentralized distributed solution. The gossip protocol is used for communication between nodes, and all nodes in the cluster can be routed to the correct node when user visits. Cluster nodes support automatic discovery of other nodes, detect faulty nodes, and ensure the application is almost not affected when the master node failed.

## Key features
- Redis compatibility
  
  Redis protocol, commands supported in tendisplus are compatible with redis.

- Persistent storage
  
  Using rocksdb as storage engine. All data is stored in rocksdb in a specific format, supporting PB-level storage capacity.

- Decentralized distributed cluster
  
  Distributed implementation like redis cluster, using a gossip protocol to propagate information between nodes.

- Horizontal scalability
  
  Data migration online between nodes. High performance and linear scalability up to 1000 nodes.

- Failover
  
  Auto detect non-working nodes, and premote slave nodes to master when a failure occurs. 
  
- Key component for Tendis Hybrid Storage Edition 
  
  Thanks to the design and internal optimization, redis and tendisplus can work together to be 
  Hybrid Storage Edition. It is suitable for KV storage scenarios, as it balances performance and cost, and greatly reduces your business operating costs by 80% in the scenarios where cold data takes up a lot of storage space. 


## Build and run

#### Requirements

* g++ (required by c++17, version >= 5.5)
* cmake (version >= 2.8.0)

#### Build

```
$ git clone http://git.code.oa.com/tencentdbforkv/tendisplus.git --recursive
$ git submodule update --init --recursive
$ mkdir bulid
$ cd build & cmake ..
$ make -j12
```

#### Run
```
$ ./build/bin/tendisplus tendisplus.conf
```

## TEST
```
$ sh ./testall.sh
```

## Performance

#### Hardware
CPU:2.50 GHz,48 core
DISK:NVMe SSD

#### 1. Commands QPS
tendisplus: workers = 56
cmd example:
./memtier_benchmark -t 20 -c 50 -s 127.0.0.1 -p 51002 --distinct-client-seed --command="set \_\_key__ \_\_data__" --key-prefix="kv_" --key-minimum=1 --key-maximum=500000000 --random-data --data-size=128 --test-time=1800
./memtier_benchmark -t 20 -c 50 -s 127.0.0.1 -p 51002 --distinct-client-seed --command="get \_\_key__" --key-prefix="kv_" --key-minimum=1 --key-maximum=500000000 --test-time=1800
./memtier_benchmark -t 20 -c 50 -s 127.0.0.1 -p 51002 --distinct-client-seed --command="incr \_\_key__" --key-prefix="int_" --key-minimum=1 --key-maximum=1000000 --test-time=1800
./memtier_benchmark -t 20 -c 50 -s 127.0.0.1 -p 51002 --distinct-client-seed --command="lpush \_\_key__ \_\_data__" --key-prefix="list_" --key-minimum=1 --key-maximum=1000000 --random-data --data-size=128 --test-time=1800
./memtier_benchmark -t 20 -c 50 -s 127.0.0.1 -p 51002 --distinct-client-seed --command="sadd \_\_key__ \_\_data__" --key-prefix="set_" --key-minimum=1 --key-maximum=1000000 --random-data --data-size=128 --test-time=1800
./memtier_benchmark -t 20 -c 50 -s 127.0.0.1 -p 51002 --distinct-client-seed --command="zadd \_\_key__ \_\_key__ \_\_data__" --key-prefix="" --key-minimum=1 --key-maximum=1000000 --random-data --data-size=128 --test-time=1800
./memtier_benchmark -t 20 -c 50 -s 127.0.0.1 -p 51002 --distinct-client-seed --command="hset \_\_key__ \_\_data__ \_\_data__" --key-prefix="hash_" --key-minimum=1 --key-maximum=1000000 --random-data --data-size=128 --test-time=1800
![image.png](/uploads/9E5C0BE69C29483584A689EFAB9DACC0/image.png)

#### 2. QPS on different payload
tendisplus: workers = 56
cmd example:
./memtier_benchmark -t 20 -c 50 -s 127.0.0.1 -p 51002 --distinct-client-seed --command="set \_\_key__ \_\_data__" --key-prefix="kv_" --command-key-pattern=R --random-data --data-size=128 --test-time=1800
./memtier_benchmark -t 20 -c 50 -s 127.0.0.1 -p 51002 --distinct-client-seed --command="get \_\_key__" --key-prefix="kv_" --command-key-pattern=R --test-time=1800
![image.png](/uploads/B458C03FE6AF4294A647098C56033B24/image.png)

## License
Tencent is pleased to support the open source community by making tendis available. 

Copyright (C) 2020 THL A29 Limited, a Tencent company.  All rights reserved. 

tendis is licensed under the GNU General Public License Version 3.0 except for the third-party components listed below. 

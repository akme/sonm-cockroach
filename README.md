# SONM CockroachDB Manager
SONM is a powerful distributed worldwide system for general-purpose computing, implemented as a fog computing structure.
Consumers of computing power in SONM get more cost-efficient solutions than cloud services., more information you can get at [docs.sonm.com](https://docs.sonm.com) and [sonm.com](https://sonm.com).

[CockroachDB](https://www.cockroachlabs.com/) is SQL database for global cloud services. It's postgesql compatible. [More information](https://www.cockroachlabs.com/docs/stable/)

This script manages to run CockroachDB distributed on SONM suppliers.


## Getting started
First of all you need to install sonmnode and sonmcli, you can do this by following [this guide](https://docs.sonm.io/getting-started/as-a-consumer)

### Prerequisites
Works only on Linux and MacOS, BSD systems not tested but may work.

Make sure you have installed all of the following prerequisites on your machine:
* sonmcli
* sonmnode
* jq
* xxd
* wget



### Installing
```
git clone https://github.com/akme/sonm-cockroach
```

## Deployment
Before deployment you need to set parameters of cluster in config.sh
* tag - cluster name used to mark BIDs and tasks
* numberofnodes - cluster size 
* ramsize - RAM size in GB that each node will have
* storagesize - storage size in GB that each node will have
* cpucores - number of CPU cores that each node will have
* sysbenchsingle - minimal CPU performance for 1 CPU core
* sysbenchmulti - minimal CPU performance for multi core systems (multithreading)
* netdownload - minimal download speed in Mbits for a deal
* netupload - minimal upload speed in Mbits for a deal
* price - price in USD for deal per hour

When all parameters are set, run:
```
./rds.sh watch
```
This will create number of orders equal to numberofnodes you've set, when any order will become a deal, script will start task on it, each next task (next deal) will join cluster created by previous tasks.

When all orders created, all deals are set and have tasks running, it will watch for deals, if deal drops then it creates new orders, wait for deal and start task on it.

### Example of output
```
$ ./rds.sh watch 
2018-08-17 23:49:21 Creating 3 order(s)
ID = 163493
ID = 163494
ID = 163496
2018-08-17 23:51:30 All set, waiting for deals
2018-08-17 23:51:30 watching cluster
2018-08-17 23:51:46 Starting task on deal 4090
Task ID:    749e08b4-bf1e-4601-99ed-640fdf1d9e33
  Endpoint: 26257/tcp: 85.119.150.185:26257
  Endpoint: 26257/tcp: 172.17.0.1:26257
  Endpoint: 8080/tcp: 85.119.150.185:8080
  Endpoint: 8080/tcp: 172.17.0.1:8080
  Network:  sonmbcfa4942
2018-08-17 23:51:58 Starting task on deal 4091
Task ID:    62d6d24d-ae2e-409d-8ac7-4ad983b08991
  Endpoint: 8080/tcp: 185.144.156.200:8080
  Endpoint: 8080/tcp: 172.17.0.1:8080
  Endpoint: 26257/tcp: 185.144.156.200:26257
  Endpoint: 26257/tcp: 172.17.0.1:26257
  Network:  sonmc44f619d
2018-08-17 23:52:13 Starting task on deal 4092
Task ID:    4e48b4e3-8d13-4a44-8d80-dcaf6ce5c04a
  Endpoint: 26257/tcp: 95.141.193.156:26257
  Endpoint: 26257/tcp: 172.17.0.1:26257
  Endpoint: 26257/tcp: 172.18.0.1:26257
  Endpoint: 8080/tcp: 95.141.193.156:8080
  Endpoint: 8080/tcp: 172.17.0.1:8080
  Endpoint: 8080/tcp: 172.18.0.1:8080
  Network:  sonm0e853006
```
## How to use
After starting a task you will get an IP and port to access.

You can view cluster dashboard at http://<any node ip>:8080

To get into SQL you can run:
```
./cockroach sql --certs-dir=certs --host <any node ip>
```

### Create user
In case you want to access cluster with Postgres protocol you need to create user:
```
./rds.sh createuser <username>
```
It will for a password and will show you how to connect with psql, like this:
```
psql -h <any node ip> -p 26257 -U <username> --set=sslmode=require
```
### Get cluster IP
You can get all cluster nodes IP addresses:
```
./rds.sh getips
```
### Changer replication factor
By default cluster runs with replication factor 3, that means any data has 3 copies, but for such dynamic environment as fog, you may want to increase this number:
```
$ ./rds.sh setreplica 5 
```
Cluster need some time to make additional copies of data, so be patient to wait until sync ends.
### Destroy cluster
When you want to stop using and destroy cluster, you just need to close all deals and cancel orders:
```
./rds.sh cancelorders
./rds.sh closedeals
```
It will stop all tasks and cleanup all created deals and orders.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details

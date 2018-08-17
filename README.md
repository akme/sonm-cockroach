# SONM Cockroach Manager

```
SONM CockroachDB Manager

./rds.sh
	stoptasks
		Stop all running tasks
	closedeals
		Close all active deals
	createuser
		Create user to access via psql
	watch
		Create orders, wait for deals, deploy tasks and watch cluster state
	setreplica
		Change number of replicas (default: 3)
```
## Example of output
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

# Deploy-a-Multi-node-Elasticsearch-Cluster-with-Docker-Compose

## Step 1. Create a docker-compose.yml file for the Elastic Stack.

Make sure Docker Engine is allotted at least 4GiB of memory.

## Step 2. Run docker-compose to bring up the three-node Elasticsearch cluster and Kibana:

```
docker-compose up
```
## Step 3. Submit a _cat/nodes request to see that the nodes are up and running:

```
curl -X GET "localhost:9200/_cat/nodes?v&pretty"
```

There is only ever one single master in a cluster, chosen among the set of master-eligible nodes.
You can either run the ```/_cat/master command``` or the ```/_cat/nodes command```.

```
curl 'localhost:9200/_cat/master?v'


OUTPUT
======

id                     ip            node
Ntgn2DcuTjGuXlhKDUD4vA 192.168.56.30 Solarr

```
Open Kibana to load sample data and interact with the cluster: http://localhost:5601.
latter command will yield the list of nodes with the master column (m for short). Nodes with m are master-eligible nodes and the one with the * is the current master.

```

curl 192.168.56.10:9200/_cat/nodes?v&h=id,ip,port,v,m

OUTPUT
========
id   ip            port version m
pLSN 192.168.56.30 9300 2.2.0   m
k0zy 192.168.56.10 9300 2.2.0   m
6Tyi 192.168.56.20 9300 2.2.0   *

```


this is how you can get the id of the master_node

```
curl -X GET "192.168.0.1:9200/_cluster/state/master_node?pretty"

{
  "cluster_name" : "logbox",
  "compressed_size_in_bytes" : 11150,
  "cluster_uuid" : "eSpyTgXbTJirTjWtPW_HYQ",
  "master_node" : "R8Gn9Km0T92H9D7TXGpX4k"
}
```


## Step 4. Open Kibana to load sample data and interact with the cluster

```
 http://localhost:5601.
 ```
 

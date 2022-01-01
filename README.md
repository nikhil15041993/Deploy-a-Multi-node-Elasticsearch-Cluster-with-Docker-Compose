# Deploy-a-Multi-node-Elasticsearch-Cluster-with-Docker-Compose

## Step 1. Create a docker-compose.yml file for the Elastic Stack.

Make sure Docker Engine is allotted at least 4GiB of memory.

## Step 2. Run docker-compose to bring up the three-node Elasticsearch cluster and Kibana:

```
docker-compose up

```
:warning: following requirements and recommendations apply when running Elasticsearch in Docker in production.

Set vm.max_map_count to at least 262144edit
The vm.max_map_count kernel setting must be set to at least 262144 for production use.

How you set vm.max_map_count depends on your platform:

Linux

The vm.max_map_count setting should be set permanently in /etc/sysctl.conf:
```
grep vm.max_map_count /etc/sysctl.conf
vm.max_map_count=262144
```
To apply the setting on a live system, run:

```
sysctl -w vm.max_map_count=262144
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
 
## Step 5. Download and install Filebeat on Remote server

Download and install Filebeat

```
DEB
====

curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.9.2-amd64.deb
sudo dpkg -i filebeat-7.9.2-amd64.deb


RPM
====
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.9.2-x86_64.rpm
sudo rpm -vi filebeat-7.9.2-x86_64.rpm

```

## Step 6. Edit the configuration

Modify /etc/filebeat/filebeat.yml to set the connection information:

```
output.elasticsearch:
  hosts: ["<es_url>"]
  username: "elastic"
  password: "<password>"
  setup.kibana:
  host: "<kibana_url>"
  
```
Where <password> is the password of the elastic user, <es_url> is the URL of Elasticsearch, and <kibana_url> is the URL of Kibana

  
## Step 7. Enable and configure the system module

```
sudo filebeat modules enable system
```  
Modify the settings in the /etc/filebeat/modules.d/system.yml file.  

## Step 8.Start Filebeat

The setup command loads the Kibana dashboards. If the dashboards are already set up, omit this command.

```
sudo filebeat setup
sudo service filebeat start
```  
  

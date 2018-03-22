# Installing elasticsearch

From: https://www.elastic.co/guide/en/elasticsearch/reference/current/_installation.html

## Dependencies

As root:

```
yum install -y java
```

## Check version on server

```
$ curl http://jasmin-es1.ceda.ac.uk:9200/
{
  "name" : "MUHt7Pz",
  "cluster_name" : "ceda",
  "cluster_uuid" : "m0u82ieWRWSGw88xuBgByg",
  "version" : {
    "number" : "6.2.2",
    "build_hash" : "10b1edd",
    "build_date" : "2018-02-16T19:01:30.685723Z",
    "build_snapshot" : false,
    "lucene_version" : "7.2.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

So, it's 6.2.2.

## Download and install

As root:

```
mkdir /usr/local/elasticsearch
chown -Rf vagrant.vagrant /usr/local/elasticsearch
```

As vagrant:

```
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.tar.gz
tar -xvf elasticsearch-6.2.2.tar.gz
cd elasticsearch-6.2.2/bin
./elasticsearch
```



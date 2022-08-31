# Elk config for your project 

<img src="1.png" width="800" height="500" />

```
https://www.elastic.co/downloads/past-releases
```

OSS is importan for your project when you need a free version
```
Logstash OSS 8.0.0
```


### opendistro
```
https://opendistro.github.io/for-elasticsearch-docs/docs/install/
```

### Operating system and JVM compatibility
```
https://opensearch.org/docs/latest/opensearch/install/compatibility

```
#### Important settings on kernel
```
https://opensearch.org/docs/latest/opensearch/install/important-settings/
```

### step1 ones
```
./ssl/ssl-gen.sh
```
### step2 up docker-compose
```
 docker-compose up -d
```

### step3 up docker-compose
```
docker-compose down 
```

You can go to this directory for config files
```
cd /var/lib/docker/volumes/opensearch_opensearch-data1-configs/_data/
```

You can change jvm.options for jvm machine
```
cat jvm.options
```


### step4 make subject for security on opensearch
```
cd ssl/
openssl x509 -subject -nameopt RFC2253 -noout -in node1.pem
openssl x509 -subject -nameopt RFC2253 -noout -in node2.pem
openssl x509 -subject -nameopt RFC2253 -noout -in node3.pem

subject=CN=opensearch-node1,OU=UNIT,O=ORG,L=TEHRAN,ST=ARIA,C=CA
```
you must run this command for all of nodes on your services finally add outpu these files
```
opensearch-1.yml 
opensearch-2.yml 
opensearch-3.yml 
```
## step5 you need copy 3 files on other location
```
cp opensearch-1.yml /var/lib/docker/volumes/opensearch_opensearch-data1-configs/_data/opensearch.yml 

cp opensearch-2.yml /var/lib/docker/volumes/opensearch_opensearch-data2-configs/_data/opensearch.yml 

cp opensearch-3.yml /var/lib/docker/volumes/opensearch_opensearch-data3-configs/_data/opensearch.yml


```

## step6 run these command on linux for copy ssl files 

```
cd ssl 
cp *.pem /var/lib/docker/volumes/opensearch_opensearch-data1-configs/_data/

cp *.pem /var/lib/docker/volumes/opensearch_opensearch-data2-configs/_data/

cp *.pem /var/lib/docker/volumes/opensearch_opensearch-data3-configs/_data/
```

## step7 change permission on linux for elk services

```
chown -R 1000.1000 /var/lib/docker/volumes/opensearch_opensearch-data1-configs/_data/
chown -R 1000.1000 /var/lib/docker/volumes/opensearch_opensearch-data2-configs/_data/
chown -R 1000.1000 /var/lib/docker/volumes/opensearch_opensearch-data3-configs/_data/

```

## step8 run this command for up all of Containers

```
docker-compose up -d
```

## step9 set security plugin configuration

```
docker exec -ti opensearch-node1 bash

ls

ls plugins/opensearch-security/





```



## step10 set security plugin configurations on container
```
sh plugins/opensearch-security/tools/securityadmin.sh -cd config/opensearch-security/ -icl -nhnv -cacert config/root-ca.pem -cert config/admin.pem -key config/admin-key.pem


```

## step11 Change admin password
```
./plugins/opensearch-security/tools/hash.sh   # add password

vi /usr/share/opensearch/config/opensearch-security/internal_users.yml # In all nodes

vi /usr/share/opensearch/config/opensearch-security/internal_users.yml


admin:
  hash: "add hash"
  reserved: true
  backend_roles:
  - "admin"
  description: "Demo admin user"


sh plugins/opensearch-security/tools/securityadmin.sh -cd config/opensearch-security/ -icl -nhnv -cacert config/root-ca.pem -cert config/admin.pem -key config/admin-key.pem # Only in master node


```

## step12 check autentication on elk on container
```
curl -XGET --insecure -u 'admin:admin' https://localhost:9200/_cluster/health?pretty
```
## open dashboard
```
http://ip:5601/app/login?nextUrl=%2F



```

## deploy elastic flow

```
cd elastiflow

cp -r * /etc/elastiflow

```

## Dashboard

```
http://172.25.26.8:5601/app/management

add tenant in Secutiry

security-dashboards-plugin#/tenants

after that you need to add index pattern  so you must click stack pattern

opensearch-dashboards/indexPatterns 


```
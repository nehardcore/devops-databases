# Задание 1
>В ответе приведите:  
>- текст Dockerfile-манифеста,  
>- ссылку на образ в репозитории dockerhub,  
>- ответ Elasticsearch на запрос пути / в json-виде.

    FROM centos:centos7

    RUN yum -y install wget; yum clean all && \
            groupadd elasticsearch && \
            adduser -g elasticsearch --home /usr/share/elasticsearch elasticsearch && \
            mkdir /var/lib/data && \
            chown -R elasticsearch:elasticsearch /var/lib/data && \
            mkdir /var/lib/logs && \
            chown -R elasticsearch:elasticsearch /var/lib/logs

    USER elasticsearch:elasticsearch

    WORKDIR /usr/share/elasticsearch

    ENV EL_VER=8.7.0

    RUN wget -q https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-${EL_VER}-linux-x86_64.tar.gz && \
            tar -xzf elasticsearch-${EL_VER}-linux-x86_64.tar.gz && \
            cp -rp elasticsearch-${EL_VER}/* ./ && \
            rm -rf elasticsearch-${EL_VER}*

    COPY elasticsearch.yml /usr/share/elasticsearch/config/

    CMD ["bin/elasticsearch"]
    
https://hub.docker.com/repository/docker/nehardcore/centos_elasticsearch/

<img width="506" alt="Screenshot 2023-04-06 at 16 22 39" src="https://user-images.githubusercontent.com/97674120/230511135-f62040e0-b4b2-4087-aec6-2a3dcbca4893.png">

# Задание 2

>Получите список индексов и их статусов, используя API, и приведите в ответе на задание.  
>Получите состояние кластера Elasticsearch, используя API.  

    cch@MBP-Costas 06-db-05-elasticsearch % curl -XPUT "http://localhost:9200/ind-1" -H 'Content-Type: application/json' -d '{"settings": {"index": {"number_of_shards": 1, "number_of_replicas": 0 }}}'
    {"acknowledged":true,"shards_acknowledged":true,"index":"ind-1"}%

    cch@MBP-Costas 06-db-05-elasticsearch % curl -XPUT "http://localhost:9200/ind-2" -H 'Content-Type: application/json' -d '{"settings": {"index": {"number_of_shards": 1, "number_of_replicas": 2 }}}'
    {"acknowledged":true,"shards_acknowledged":true,"index":"ind-2"}%

    cch@MBP-Costas 06-db-05-elasticsearch % curl -XPUT "http://localhost:9200/ind-3" -H 'Content-Type: application/json' -d '{"settings": {"index": {"number_of_shards": 2, "number_of_replicas": 4 }}}'
    {"acknowledged":true,"shards_acknowledged":true,"index":"ind-3"}%
    
    cch@MBP-Costas 06-db-05-elasticsearch % curl -XGET "http://localhost:9200/_cat/indices?v"
    health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
    green  open   ind-1 JJ_zd84DTlykYqR5sneIqA   1   0          0            0       225b           225b
    yellow open   ind-3 Ht4hJZnXRoC2omjc2Bftug   2   4          0            0       450b           450b
    yellow open   ind-2 2mk5oC8mQ2eZdqgsqs48TA   1   2          0            0       225b           225b
    
    cch@MBP-Costas 06-db-05-elasticsearch % curl "http://localhost:9200/_cluster/health?pretty"
    {
      "cluster_name" : "netology_cluster_test",
      "status" : "yellow",
      "timed_out" : false,
      "number_of_nodes" : 1,
      "number_of_data_nodes" : 1,
      "active_primary_shards" : 4,
      "active_shards" : 4,
      "relocating_shards" : 0,
      "initializing_shards" : 0,
      "unassigned_shards" : 10,
      "delayed_unassigned_shards" : 0,
      "number_of_pending_tasks" : 0,
      "number_of_in_flight_fetch" : 0,
      "task_max_waiting_in_queue_millis" : 0,
      "active_shards_percent_as_number" : 28.57142857142857
    }
    
  >Как вы думаете, почему часть индексов и кластер находятся в состоянии yellow?  
  "unassigned_shards" : 10,
  
  >Удалите все индексы.  
  
    cch@MBP-Costas 06-db-05-elasticsearch % curl -XDELETE http://localhost:9200/ind-1
    {"acknowledged":true}%
    cch@MBP-Costas 06-db-05-elasticsearch % curl -XDELETE http://localhost:9200/ind-2
    {"acknowledged":true}%
    cch@MBP-Costas 06-db-05-elasticsearch % curl -XDELETE http://localhost:9200/ind-3
    {"acknowledged":true}%
    
    
# Задание 3


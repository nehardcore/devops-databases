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

>Создайте директорию {путь до корневой директории с Elasticsearch в образе}/snapshots.  
>Используя API, зарегистрируйте эту директорию как snapshot repository c именем netology_backup.  
>Приведите в ответе запрос API и результат вызова API для создания репозитория.  

    cch@MBP-Costas ~ % curl -X PUT http://localhost:9200/_snapshot/netology_backup -H 'Content-Type: application/json' -d' { "type": "fs", "settings": { "location": "/var/lib/snapshot"}}'
    {"acknowledged":true}%
    
>Создайте индекс test с 0 реплик и 1 шардом и приведите в ответе список индексов.  
  
    cch@MBP-Costas ~ % curl -XPUT "http://localhost:9200/test" -H 'Content-Type: application/json' -d '{"settings": {"index": {"number_of_shards": 1, "number_of_replicas": 0 }}}'
    {"acknowledged":true,"shards_acknowledged":true,"index":"test"}%
    
>Создайте snapshot состояния кластера Elasticsearch.

    cch@MBP-Costas ~ % curl -XPUT 'http://localhost:9200/_snapshot/netology_backup/snapshot-20230406?wait_for_completion=true'
    {"snapshot":{"snapshot":"snapshot-20230406","uuid":"WMohqNZeRfy9GphjXPlwcQ","repository":"netology_backup","version_id":8070099,"version":"8.7.0","indices":["test"],"data_streams":[],"include_global_state":true,"state":"SUCCESS","start_time":"2023-04-07T01:36:02.105Z","start_time_in_millis":1680831362105,"end_time":"2023-04-07T01:36:02.505Z","end_time_in_millis":1680831362505,"duration_in_millis":400,"failures":[],"shards":{"total":1,"failed":0,"successful":1},"feature_states":[]}}%
    
>Приведите в ответе список файлов в директории со snapshot.  

    cch@MBP-Costas ~ % docker exec -it es sh -c "ls /var/lib/snapshot/ -l"
    total 36
    -rw-r--r-- 1 elasticsearch elasticsearch   593 Apr  7 01:36 index-0
    -rw-r--r-- 1 elasticsearch elasticsearch     8 Apr  7 01:36 index.latest
    drwxr-xr-x 3 elasticsearch elasticsearch  4096 Apr  7 01:36 indices
    -rw-r--r-- 1 elasticsearch elasticsearch 18835 Apr  7 01:36 meta-WMohqNZeRfy9GphjXPlwcQ.dat
    -rw-r--r-- 1 elasticsearch elasticsearch   315 Apr  7 01:36 snap-WMohqNZeRfy9GphjXPlwcQ.dat
    
>Удалите индекс test и создайте индекс test-2. Приведите в ответе список индексов.

    cch@MBP-Costas ~ % curl -XGET 'localhost:9200/_cat/indices?v'
    health status index  uuid                   pri rep docs.count docs.deleted store.size pri.store.size
    green  open   test-2 S0ahypBkTT6EBSdo6r6Ktg   1   0          0            0       225b           225b 
    
>Восстановите состояние кластера Elasticsearch из snapshot, созданного ранее.  
>Приведите в ответе запрос к API восстановления и итоговый список индексов.
    
    cch@MBP-Costas ~ % curl -X POST 'http://localhost:9200/_snapshot/netology_backup/snapshot-20230406/_restore?pretty'

    {
      "accepted" : true
    }


    cch@MBP-Costas ~ % curl -XGET 'localhost:9200/_cat/indices?v'
    health status index  uuid                   pri rep docs.count docs.deleted store.size pri.store.size
    green  open   test-2 S0ahypBkTT6EBSdo6r6Ktg   1   0          0            0       225b           225b
    green  open   test   41xpvezYQ6OIzt871sILug   1   0          0            0       225b           225b


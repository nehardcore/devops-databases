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
>Как вы думаете, почему часть индексов и кластер находятся в состоянии yellow?

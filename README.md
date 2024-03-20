## <h1>Application Spring Boot ElasticSearch</h1>
***
<img src="https://user.oc-static.com/upload/2017/10/10/15076639807937_Elasticsearch-Logo-Color-V.jpg.png" alt="drawing" width="280px"/>

## Informations Générales
***
Mise en place d'un moteur de recherche avec Spring-boot
## Technologies
***
Technologies utilisées:
* Java 17 
* Maven 3.6
* Spring-boot: 3.2.3
## Instalation
***
Instalation et lancement du service elasticsearch sur le port 9200
```
version: '2.2'
services:

  elasticsearch-app:
    build: .
    container_name: elasticsearch-app
    ports:
      - "18080:8080"
      - "18089:8089"
    depends_on:
      - elasticsearch
    networks:
      - elknet

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.2.0
    container_name: elasticsearch
    environment:
      - node.name=elasticsearch
      - discovery.type=single-node
#      - discovery.seed_hosts=elasticsearch
#      - cluster.initial_master_nodes=elasticsearch
      - cluster.name=docker-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"

    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - esdata1:/usr/share/elasticsearch/data
    ports:
      - 9300:9300
      - 9200:9200
    networks:
      - elknet

  kibana:
    image: docker.elastic.co/kibana/kibana:7.2.0
    container_name: kibana
    environment:
      ELASTICSEARCH_URL: "http://elasticsearch:9300"
    ports:
      - 5601:5601
    networks:
      - elknet

  logstash:
    image: docker.elastic.co/logstash/logstash:7.2.0
    container_name: logstash
    command: logstash -f /etc/logstash/conf.d/logstash.conf
    volumes:
      - ./config:/etc/logstash/conf.d
    ports:
      - "5000:5000"
    networks:
      - elknet

volumes:
  esdata1:
    driver: local

networks:
  elknet:

```
Lancement de l'application Spring-boot<br>
```
$ mvn  clean
$ mvn spring-boot:run
```
Le service est accessible sur http://localhost:8080

## FAQs
***
**Aucune Remarque**

# Actividad Graylog

Graylog es un sistema de manejo de logs. Este sistema permite la búsqueda y análisis de registros en tiempo real de cualquier componente de una infraestructura.

A continuación se muestra el fichero docker compose utilizado para crear una estructura con ElasticSearch, Mongo, Graylog, Mysql y Wordpress.

Fichero `docker-compose.yml`:

``` yml
version: '3'
services:

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch-oss:6.8.2
    environment:
      - http.host=0.0.0.0
      - transport.host=localhost
      - network.host=0.0.0.0
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    deploy:
      resources:
        limits:
          memory: 1g
    volumes:
      - elasticsearch:/usr/share/elasticsearch/data
    networks:
      - practica_network

  mongo:
    image: mongo:3
     ports:
      - 27017:27017
    volumes:
      - mongo:/data/db 
    networks:
      - practica_network

  graylog:
    image: graylog/graylog:3.1
    environment:
      - GRAYLOG_PASSWORD_SECRET=somepasswordpepper
      - GRAYLOG_ROOT_PASSWORD_SHA2=8c6976e5b5410415bde908bd4dee15dfb167a9c873fc4bb8a81f6f2ab448a918
      - GRAYLOG_HTTP_EXTERNAL_URI=http://127.0.0.1:9001/
    networks:
      - practica_network
    depends_on:
      - mongo
      - elasticsearch
    ports:
      - 9001:9000
      - 1514:1514
      - 1514:1514/udp
      - 12201:12201
      - 12201:12201/udp

  mysql:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    volumes:
      - mysql:/var/lib/mysql
    networks:
      - practica_network
    ports:
      - 3306:3306
  
  wordpress:
    depends_on:
       - mysql
    image: wordpress:latest
    restart: on-failure
    environment:
        WORDPRESS_DB_HOST: mysql:3306
        WORDPRESS_DB_USER: wordpress
        WORDPRESS_DB_PASSWORD: wordpress
        WORDPRESS_DB_NAME: wordpress
    networks:
      - practica_network
    ports:
      - 9090:80

networks:
  practica_network:
    driver: bridge

volumes:
  elasticsearch:
    driver: local
  mongo:
    driver: local
  graylog:
    driver: local
  mysql:
    driver: local
```
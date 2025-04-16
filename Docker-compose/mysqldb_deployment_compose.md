### ***Using the Docker compose deploy mysql server***

docker-compose.yaml
```
name: ums-stack
services:
  mysql:
    container-name: ums_mysqldb
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_PASSWORD="test12345"
      MYSQL_DATABASE= webappdb
    ports:
      "3061"3061"
    volumes"
      - mysql1:/var/lib/mysql
volumes:
  mysql1:
```

To creating & running the mysql
> docker compose up -d

To see the logs 
> docker compose logs mysql


***Adding metadata to the volumes***
```
name: ums-stack
services:
  mysql:
    container-name: ums_mysqldb
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_PASSWORD="test12345"
      MYSQL_DATABASE= webappdb
    ports:
      "3061"3061"
    volumes"
      - mysql1:/var/lib/mysql
volumes:
  mysql1:
     name: ums_mysql_db
     driver: local
     labels:
        project: "ums_stack"
        component: "mysql-database"
        purpose: "persists-storage"
```


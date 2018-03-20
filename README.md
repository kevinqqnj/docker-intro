# docker-intro


## postgresql
https://ryaneschinger.com/blog/dockerized-postgresql-development-environment/

### Solution1: use Volume (survive after reboot)
docker volume create pgdata

docker-compose.yml:
```
   version: "3"
   services:
     db:
       image: postgres
       environment:
         - POSTGRES_USER=postgres
         - POSTGRES_PASSWORD=postgress
         - POSTGRES_DB=postgres
       ports:
         - "5433:5432"
       volumes:
         - pgdata:/var/lib/postgresql/data
       networks:
         - suruse
   volumes: 
     pgdata:
```
docker run --name local-postgres -p 5432:5432 -d -v pgdata:/var/lib/postgresql/data postgres:9

### Solution2: The Data Volume Container (container could not survive?)
docker create -v /var/lib/postgresql/data --name pgdata busybox
docker create -v $(pwd)/pgdata:/var/lib/postgresql/data --name pgdata busybox
### The Postgresql Container
docker run --name local-postgres -e POSTGRES_PASSWORD=password -d --volumes-from pgdata postgres:9
docker run --name local-postgres -p 5432:5432 -e POSTGRES_PASSWORD=password -d --volumes-from pgdata --rm postgres:9
### connect from client
psql -h localhost -p 5432 -U postgres

Try again:
docker rm -f $(docker ps -aq)
docker create -v /var/lib/postgresql/data --name pgdata pgdata-demo:0

docker run -it --link local-postgres --rm postgres:9 sh -c 'exec psql -h "$POSTGRES_PORT_5432_TCP_ADDR" -p "$POSTGRES_PORT_5432_TCP_PORT" -U postgres'

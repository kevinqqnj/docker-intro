# docker-intro


## postgresql
https://ryaneschinger.com/blog/dockerized-postgresql-development-environment/

### The Data Volume Container
docker create -v /var/lib/postgresql/data --name pgdata busybox
### The Postgresql Container
docker run --name local-postgres -e POSTGRES_PASSWORD=password -d --volumes-from pgdata postgres:9

### save pgdata to new image
docker commit 8764ef4febf4 pgdata-demo:0


Try again:
docker rm -f $(docker ps -aq)
docker create -v /var/lib/postgresql/data --name pgdata pgdata-demo:0


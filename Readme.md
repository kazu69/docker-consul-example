## docker-consul-example

> An example of consul using docker-compose.
> It is possible to try consul and [consul-template](https://github.com/hashicorp/consul-template).

## Service Composition

```
# server
consul --
         `- consul1 (leader) 172.16.238.10
         `- consul2          172.16.238.11
         `- consul3          172.16.238.12
# agent
nginx  --
         `- nginx1           172.16.238.13
         `- nginx2           172.16.238.14
         `- nginx3           172.16.238.15
```

##3 setup

```sh
$ docker-compose up
```

#### join nodes to cluster

Example of joining nginx 1 to a cluster.
For nginx 2, ngin x 3, execute ngin x 1 with the name of each node.

```sh
# nginx1
# copy config file
$ docker cp config/nginx1 nginx1:/etc/consul.d
# join a cluster
$ docker exec -d -t nginx1 consul agent -config-file=/etc/consul.d/nginx1
# exec tcp check script
$ docker exec -d -t nginx1 bash -c './http_request nginx1'
```

#### consul-template example

```sh
$ docker-compose up

$ docker cp config/nginx1 nginx1:/etc/consul.d
$ docker exec -d -t nginx1 consul agent -config-file=/etc/consul.d/nginx1
$ docker exec -d -t nginx1 bash -c './http_request nginx1'

# template watch nodes
docker exec -it nginx1 /bin/bash
consul-template -consul-addr 127.0.0.1:8500 -template "/etc/consul-template/config.d/templates/hosts.ctmpl:/hosts"

# add nodes
$ docker cp config/nginx2 nginx2:/etc/consul.d
$ docker exec -d -t nginx2 consul agent -config-file=/etc/consul.d/nginx2
$ docker exec -d -t nginx2 bash -c './http_request nginx2'

# update template
# consul1
172.16.238.10 consul1

# consul2
172.16.238.11 consul2

# consul3
172.16.238.12 consul3

# nginx1
172.16.238.13 nginx1

# nginx2
172.16.238.14 nginx2
```

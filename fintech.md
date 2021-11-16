# How to make fintech and portal connect with

## Update Portal project
```
//1. check the docker network list. and find the name
docker network list
---
NETWORK ID     NAME                         DRIVER    SCOPE
7393c63135b4   amazeeio-network             bridge    local
00bd133461ee   bridge                       bridge    local
d7e9e71f4e73   dollar-for-schools_default   bridge    local
744a2e33df72   fintech-api_default          bridge    local
df619815d61d   fltnz_default                bridge    local
8150140b040b   host                         host      local
48e93829e3db   none                         null      local
4bba1eacdaec   zagga-au-portal_default      bridge    local
7b39e855045b   zagga-nz-portal_default      bridge    local


//2. find the IP gateway
docker network inspect fintech-api_default 

---
 "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.25.0.0/16",
                    "Gateway": "172.25.0.1"
                }
            ]
        },


//3. update IP address in docker-compose.override.yml
---
version: '2.3'

services:

  cli:
    environment:
        LAGOON_ROUTE: http://zagga-nz-portal.docker.amazee.io
    extra_hosts:
      - "fintech.docker.amazee.io:172.25.0.1"

  nginx:
    environment:
        LAGOON_ROUTE: http://zagga-nz-portal.docker.amazee.io
    extra_hosts:
      - "fintech.docker.amazee.io:172.25.0.1"

  php:
    environment:
        LAGOON_ROUTE: http://zagga-nz-portal.docker.amazee.io
    extra_hosts:
      - "fintech.docker.amazee.io:172.25.0.1"

  mariadb:
    environment:
      LAGOON_ROUTE: http://zagga-nz-portal.docker.amazee.io

  advancedqueue:
    environment:
        LAGOON_ROUTE: http://zagga-nz-portal.docker.amazee.io
    extra_hosts:
      - "fintech.docker.amazee.io:172.25.0.1"

```

## Update fintech Project
```
do the same step 1, 2, 3

```

## Both
after step 1,2,3 re-up container
```
docker-compose up -d --f 
```
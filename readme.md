# Claims
Use cases for MarkLogic to get a 360 degree view of claims

## Prerequisites
- MarkLogic Server 10.0.2 (https://developer.marklogic.com or https://hub.docker.com/_/marklogic)
- Data Hub Framework 5.1 (https://github.com/marklogic/marklogic-data-hub)
- Data Explorer

## Install
### Install using Docker
```sh
docker run -it -d \
-p 8000-8100:8000-8100 \
-p 9000-9050:9000-9050 \
-p 80:80 -p 3000:3000 \
-v [[your path]]:/mydhfproject \
--name claims \
mlregistry.marklogic.com/datahub/datahubwebwithexplorer:5.1.0-rc1
```

### Start MarkLogic, Quickstart and Explorer
```sh
docker exec claims mladmin start
docker exec claims init-marklogic admin admin
docker exec claims start-quickstart 
// For verbose logging of DHF go into the docker:
// docker exec -it claims bash 
// - java -jar /usr/local/bin/datahubweb.war
docker exec claims start-explorer
```
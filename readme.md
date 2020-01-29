# Ethias - 
Use cases for MarkLogic to get a 360 degree view of claims
1) Ability to ingest data as is (xml message / Oracle feed)  and expose a common object (Claim NUmber + Place of Occurrence (address+geo) + Vehicle Infos) 
2) Show the agility to add a new field to an entity
3) MDM (match existing Person object based on attributes like FirstName/LasteName/ Adress)
4) Supply data to salesforce (REST API) and making it available in a BI tool
- Visualisation is optional


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
-v /Users/tmeijrin/Documents/MarkLogic/Projects/:/mydhfproject \
--name ethias \
mlregistry.marklogic.com/datahub/datahubwebwithexplorer:5.1.0-rc1
```

### Start MarkLogic, Quickstart and Explorer
```sh
docker exec ethias mladmin start
docker exec ethias init-marklogic admin admin
docker exec ethias start-quickstart 
// For verbose logging of DHF go into the docker:
// docker exec -it ethias bash 
// - java -jar /usr/local/bin/datahubweb.war
docker exec ethias start-explorer
```

```sh
vi ~/.bashrc
add "export JAVA_HOME=/usr"
```

### Follow pipes installation instructions
```sh
https://project.marklogic.com/repo/projects/FRAN/repos/visual-programming-plugin/browse
java -jar pipes-xyz.jar --mlUsername=myusername --mlPassword=mypassword --deployBackend=true

```

### Additionally with the xml files, split the xml
```sh
jq -cM 'range(0; length; 1) as $i | .[$i:$i+1]' claims.xml | split -l 1 -a 3 - ./claim_

for f in ./*
do
  sed 's/^.//;s/.$//' $f > ${f}_new.xml
  rm $f
done
```


### Mapping conversions
// country = if (CDPAYSI="B") then "BE" else "NL"
// lossdate = concat(substring(DACLOSI,6,2),"-",substring(DACLOSI,5,1),"-",substring(DACLOSI,0,5))
// noticedate = concat(substring(DADCLSI,6,2),"-",substring(DADCLSI,5,1),"-",substring(DADCLSI,0,5)) 
// createdate = concat(substring(DAOUVSI,6,2),"-",substring(DAOUVSI,5,1),"-",substring(DAOUVSI,0,5))
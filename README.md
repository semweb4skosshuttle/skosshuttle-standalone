# Skos Shuttle standalone
Welcome to **SKOS Shuttle** - RDF based Thesaurus Management - https://skosshuttle.ch 

The provided docker-compose file orchestrates a complete SKOS Shuttle standalone (in the following simply "SKOS Shutlle") for your site environment. It differs from the online service version, since it can process just one tenant.

**SKOS Shuttle is an application to maintain and use RDF/SKOS thesauri on the web.**

All you need is a login to the (private) repository ch.semweb.ch:15476 - ask here for more informations

The provided docker compose allows you to orchestrate a SKOS Shuttle working totally on HTTP or on HTTPS (SSL).


# Components of SKOS Shuttle standalone

## sksdb - the database
Mysql together with an opportune database schema is initialized to run at port 3306
This contains several databases which are the basis to work with SKOS Shuttle

## sksblazegraph - the triplestore
Tomcat image with a Blazegraph module and a ready-to-use journal

## sks - SKOS Shuttle itself
Tomcat application complete with API ready-to be used after correct initialization.

# Preliminaries
Please make sure your docker is running, you have docker-compose installed and you are logged in into the repository ch.semweb:15476 - then unzip the file and move it to your place.

Although the docker-compose-http.yml (HTTP-only) version should run immediately, it is a good way first to start one component, then the other and see whether they are communicating. The suggested sequence is:

1) Start (only) sksdb 
2) Start (only) sksblazegraph
3) Start all three

You will probably have to review the **file .env** (since it contains important default values) and to change it according with your needs and infrastructure (You find this file in the zipped file) - the first thing you might do is to adapt $DOCKERHOST to tell to the compose instruction the IP of your docker host machine.

# SKOS Shuttle HTTP only

docker-compose-http.yml describes how to run a SKOS Shuttle using HTTP communication channels. Actually you can work very well already with this HTTP only version (slightely faster), if you do not have to.

To orchestrate SKOS Shuttle in HTTP, just copy docker-compose-http.yml to docker-compose.yml and call "docker-compose up" to orchestrate the predisposed containers. 

# SKOS Shuttle HTTPS

For all those cases where your network requires SSL, docker-compose-ssl.yml describes how to run a SKOS Shuttle using HTTP communication channels. In order to do configure SSL you need a keystore for tomcat and the corrensponding password. 
Complete this information as env variables in the docker-compose file the and provide the keystore as a volume file as suggested by the compose file.

To orchestrate SKOS Shuttle in SSL, copy docker-compose-ssl.yml to docker-compose.yml and run "docker-compose up"

# Notes

1) SKOS Shuttle can work with any of the tested triplestores accessible from your network. 
2) The module sks generates under its volume a ready-to-use database to be reused at the next start
3) The moduls sksblazegraph generates under its volume a Blazegraph journal in order to persist changes for the next start.
4) There is no SSL for the database (mysql) and no load-balancing for sks / sksblazegraph (being the latters tomcats) - mysql (should not be exposed, therefore) needs no exposed ports and hence no SSL to protect it. Docker swarms technology can be used to balance the present tomcat modules (sks and sksblazegraph) opportunely. 

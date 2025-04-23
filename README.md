# Open MCT QuickStart

This repository contains a quick way to get started with Open MCT, integrated with:
* [Open MCT](https://nasa.github.io/openmct/)
* [CouchDB](https://couchdb.apache.org/)
* [YAMCS](https://yamcs.org/)
* [Apache HTTP Server](https://httpd.apache.org/)

## Prerequisites

* [Docker](https://docs.docker.com/get-docker/)

## Getting Started

1. Clone this repository
2. cd to the repository directory (usually `openmct-quickstart`)
3. Run `docker compose up`
4. Wait a bit for the containers to start ⏱️
5. Open a browser to http://localhost:8040
6. Enter the username/password `testuser`/`NasaIsCool!`

## Nuts and Bolts

The `docker-compose.yml` file in this repository defines a set of containers that work together to provide a complete Open MCT environment. The containers are:
* `openmct` - Builds the Open MCT web application into a shared volume (and quits)
* `couchdb` - The CouchDB database used by Open MCT to persist objects created by the operator.
* `yamcs` - The YAMCS telemetry & commanding server used by Open MCT to retrieve telemetry data.
* `simulator` - A simple python simulator that generates telemetry data for YAMCS to serve.
* `apache` - The Apache HTTP server used to serve the Open MCT web application, and to proxy requests to YAMCS and CouchDB.

Hosted websites are available at the following URLs:
* The OpenMCT web application is served from http://localhost:8040
* The YAMCS web application is served from http://localhost:8040/yamcs
* The Apache server status is served from http://localhost:8040/server-status
* The CouchDB web application is served from http://localhost:8040/couchdb/_utils (with username `admin` and password `password`)

## Diagram

```mermaid
graph TD
    A[Apache HTTP Server] -- serves --> B[Open MCT Web Application]
    A -- proxies --> C[CouchDB Database]
    A -- proxies --> D[YAMCS Telemetry & Commanding Server]
    E[Python Simulator] -- generates telemetry data --> D
    F[Docker Compose] -- orchestrates --> A
    F -- orchestrates --> B
    F -- orchestrates --> C
    F -- orchestrates --> D
    F -- orchestrates --> E

    B -- "HTTP Traffic & JSON Storage" --> C
    D -- "HTTP Traffic & JSON Storage" --> B

    click B "http://localhost:8040" "OpenMCT Web Application"
    click C "http://localhost:8040/couchdb/_utils" "CouchDB"
    click D "http://localhost:8040/yamcs" "YAMCS"
    click A "http://localhost:8040/server-status" "Apache Server Status"
```

## Frequently Asked Questions
### How do I modify the Open MCT configuration, for example, to reference a different Yamcs instance?
From the `openmct-quickstart` directory on your host machine:
1. Edit `openmct/openmct-quickstart.js`
2. `docker compose down -v openmct`
3. `docker compose up openmct`
The key thing to note here is the `-v` flag. It is necessary to recreate a volume that contains the Open MCT configuration, otherwise it will continue to use a cached version even after you change it.
### I want to expose Yamcs ports to the outside world so that I can feed telemetry in from an external system
From the `openmct-quickstart` directory on your host machine:
1. Edit `docker-compose.yaml`
2. Add a `ports` block to the `yamcs` section per the Docker guidance on this topic - https://docs.docker.com/get-started/docker-concepts/running-containers/publishing-ports/#use-docker-compose
eg.
```
ports:
    - "10015:10015/udp"
```

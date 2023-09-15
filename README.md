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
3. Run `docker-compose up`
4. Wait a bit for the containers to start ⏱️
5. Open a browser to http://localhost:8040

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

## Authentication

This branch includes the basic configuration necessary to integrate Open MCT with Yamcs based authentication. To run with authentication enabled just follow the instructions above.

There are several configuration files that enable authentication:

* `apache/openmct.conf` - Apache configuration that inserts a necessary header to prompt the browser to show a login dialog
* `yamcs/security.yaml` - Enables the `YAML AuthModule` which authenticates users defined in a local `users.yaml` file. In principle Open MCT auth should also work with the Yamcs LDAP auth module, but this has never been tested.
* `yamcs/users.yaml` - Yamcs user definitions. See https://docs.yamcs.org/yamcs-server-manual/security/authmodules/yaml/
* `yamcs/roles.yaml` - Yamcs role definitions. See https://docs.yamcs.org/yamcs-server-manual/security/configuration/

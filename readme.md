# Personal Server 

## Architecture

### At the moment

````mermaid
flowchart LR
    User
    subgraph server
        S3[Minio]
        Reg[Docker Registy]
    end
    User -- port 9000 --> S3
    User -- port 9090 - IHM --> S3
    User -- port 5000 --> Reg
    Reg -- Storage --> S3
```` 

### Goal

````mermaid
flowchart LR
    User
    subgraph server
        RP[NgInx Reverse Proxy]
        S3[Minio]
        Reg[Docker Registy]
    end
    User -- https --> RP
    RP -- port 9000 --> S3
    RP --> Reg
    Reg -- Storage --> S3
```` 

## Server Spec

* Server OS : Ubuntu
* CPU : 4 vCPU 
* Memory : 8Go
* Disk : 200Go

## Installation

### Requirement

* Docker, docker compose

###

* Set a user name and a password for minio in `minio/config.env`.
  * Set the same user & password in  the registry ``config.yml`` and in the container `createbuckets` entrypoint.
* Launch the ``docker-compose.yml`` with ``docker compose -f docker-compose.yml up -d``

Possible to launch portainer with ``docker compose -f portainer-compose.yml up -d``.

## Todos

* [x] Deploy a S3, with a bucket initiate
  * [ ] Limit the container resources
  * [ ] Fix secret error for createbuckets ``WARN[0000] The "MINIO_ROOT_PASSWORD" variable is not set. Defaulting to a blank string. ``
* [x] Deploy a Docker Registry
  * [ ] Modify the configuration so the S3 user/pwd is not in the config file
  * [ ] Limit the container resources
* [ ] Deploy a Reverse proxy (NgInx ?) To route every call to docker registry or S3
* [ ] Configure a supervision (to be defined)
* [ ] Configure the containers:
  * [ ] Set resources limits,
  * [ ] Set Priority
* [ ] Configure the system
  * [ ] Add firewall rules.

### Supervision

Look at Grafana Loki
Look at netdata
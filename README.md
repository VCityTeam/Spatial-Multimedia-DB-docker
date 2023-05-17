# Spatial-Multimedia-DB-docker

This repository holds the docker based containerization of
[Spatial Multimedia database](https://github.com/VCityTeam/Spatial-Multimedia-DB)
that is used to deploy a database dedicated to the storage of multimedia
documents oriented towards a geographical 3D environment.

## General information

**WARNING** : The container defined by the `Spatial-Multimedia-DB-context/`
context requires a [Postgres database](https://www.postgresql.org/) to run
properly. Thus, when running this container you MUST also have another container
installed and running `postgres` (or alternatively running on the host, which
is not a recommandable practice).
This underlying `postgres` server can possibly be run through another
`docker run` command or launched through some docker-compose setup.

## How to use the docker container

### docker command only (as opposed to docker compose) 

```bash
# Launch a postgres server on some alternate port (to the default one that is
# 5432, in order to avoid a possible collision with a postgres server running
# natively on host) 
docker run -d -e POSTGRES_PASSWORD=mypass -e POSTGRES_USER=myuser -e POSTGRES_DB=somedb -p 5444:5432 postgres:15
docker ps | grep postgres     # to assert the server is running  
```

```bash
# Retrieve your host IP number e.g. on OSX and when on Wifi
ipconfig getifaddr en0    # that yields e.g. 10.42.206.60
```


```bash
docker build -t smdb ./Spatial-Multimedia-DB-context/
docker run \
  -e POSTGRES_SMDB_ORDBMS=postgresql \
  -e POSTGRES_HOST=10.42.206.60      \
  -e POSTGRES_PORT=5444              \
  -e POSTGRES_USER=myuser            \
  -e POSTGRES_PASSWORD=mypass        \
  -e POSTGRES_DB_NAME=somedb         \
  -e SPATIAL_MULTIMEDIA_DB_PORT=8997                 \
  -e SPATIAL_MULTIMEDIA_DB_ADMIN_EMAIL=admin1@udv.fr \
  -e SPATIAL_MULTIMEDIA_DB_ADMIN_FIRST_NAME=admin1   \
  -e SPATIAL_MULTIMEDIA_DB_ADMIN_LAST_NAME=somename  \
  -e SPATIAL_MULTIMEDIA_DB_ADMIN_PASSWORD=password1  \
  -e SPATIAL_MULTIMEDIA_DB_ADMIN_USERNAME=admin1     \
  -t smdb
```

and use your browser (or a consuming application) to interact
`http://localhost:<SPATIAL_MULTIMEDIA_DB_PORT>` (by default 
`http://localhost:5000`) where you should find a webpage confirming that your
deployment was successful.

### Using docker-compose (easier, supposedly)
Refer to the [`Example` sub-directory](Example/Readme.md)






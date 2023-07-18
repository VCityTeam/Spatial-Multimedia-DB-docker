# Using Spatial-Multimedia-DB through docker-compose

<!-- TOC -->

- [Running and halting the Spatial-Multimedia-DB SMDB server](#running-and-halting-the-spatial-multimedia-db-smdb-server)
- [Testing things](#testing-things)
- [Container's known issues](#containers-known-issues)
  - [Containers not awaiting each other](#containers-not-awaiting-each-other)
- [Developers](#developers)
  - [Developing SMDB server python code](#developing-smdb-server-python-code)

<!-- /TOC -->

## Running and halting the Spatial-Multimedia-DB (SMDB) server
```bash
cd Example
```

Edit the [`.env` file](.env) and adapt it to your needs.
Then launch the service with

```bash
docker-compose up -d
```

Consume the database through some web based application that is connect to 
`http://localhost:<SPATIAL_MULTIMEDIA_DB_PORT>` (e.g. `http://localhost:8997` 
if you used the default [`.env` configuration file](./.env)) where you should 
see a webpage confirming that your deployment was successful.

Eventually shut down the service
```bash
docker-compose down
```

## Testing things
Once the server(s) are up, one can request the Spatial-Multimedia-DB (SMDB).
For example, if you used the default [`.env` configuration file](./.env) then
you can try e.g.

```bash
curl -X 'POST'   'http://localhost:8997/login'   \
     -H 'accept: application/json' -H 'Content-Type: multipart/form-data'  \
     -F 'username=admin1' -F 'password=password1'
```

that should return a token, or

```bash
curl -X 'GET'   'http://localhost:8997/document'   \
     -H 'accept: application/json'   -H 'Content-Type: multipart'
```

that should return the list of documents (empty by default i.e. `[]`).

## Container's known issues 

### Containers not awaiting each other

Some dockers need to wait for another docker to be up and running so they
can complete their task properly (refer to the `depends_on:` directive
within the [`docker-compose.yml` file](./`docker-compose.yml). In normal
circumstances they do so. 
However, on some occasions, it has been observed that the SpatialMultimediaDB
container will start running while the `postgres` container has not yet finished
to initialized the database, resulting in the SpatialMultimediaDB being unable
to connect to the database. If a message similar to the one shown below gets
displayed, then relaunching the demo should solve the issue. 

```bash
extended_doc_api | /api
extended_doc_api | Trying to connect to Database...
extended_doc_api | Config :  postgresql://postgres:password@postgres:5432/extendedDoc
extended_doc_api | Connection failed (psycopg2.OperationalError) could not connect to server: Connection refused
extended_doc_api |      Is the server running on host "postgres" (172.22.0.2) and accepting
extended_doc_api |      TCP/IP connections on port 5432?
extended_doc_api | 
extended_doc_api | (Background on this error at: http://sqlalche.me/e/e3q8)
```

If it doesn't, check that your .env file is properly configured.

## Developers

### Developing SMDB server (python code)

The current repository relies on SMDB's server code as pointed by 
`Spatial-Multimedia-DB-context/Dockerfile`. But debugging that server code when 
ran within a docker container might be a harder context that from the CLI (or
through a IDE accessing local files). For the rest of this section, let us
assume that `SMDB-DOCKER` is an environment variable pointing to the directory
where this repository was checked out.

In order to debug SMDB's code locally 
[install SMDB](https://github.com/VCityTeam/Spatial-Multimedia-DB/blob/master/doc/Install.md#manual-install), configure it and run it as follows 

```bash
cd `git rev-parse --show-toplevel`     # That is Spatial-Multimedia-DB dir
mv .env .env.set_aside
cp ${SMDB-DOCKER}/
wget https://raw.githubusercontent.com/VCityTeam/Spatial-Multimedia-DB-docker/master/Example/.env
echo 'KC_REALM=vcity' >> .env
echo 'KC_SERVER_URL=http://localhost/${KEYCLOAK_PORT}' >> .env
echo 'KC_CLIENT_ID=
source venv/bin/activate
(venv) 

```
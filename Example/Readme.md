# Using Spatial-Multimedia-DB through docker-compose

```bash
cd Example
```

Edit the [`.env` file](.env) and adapt it to your needs.
Then launch the service with

```bash
docker-compose up -d
```

Consume the database through some web based application
that is connect to `http://localhost:<SPATIAL_MULTIMEDIA_DB_PORT>`
where you should see a webpage confirming that your deployment was successful.

Eventually shut down the service
```bash
docker-compose down
```

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

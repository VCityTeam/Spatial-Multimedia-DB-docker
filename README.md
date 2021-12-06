# Spatial-Multimedia-DB-docker

## General information

- IMPORTANT : This docker requires a [Postgres database](https://www.postgresql.org/) to run properly. You should have one currently installed and running, possibly in another docker or setup in your docker-compose for example. If you don't have one currently up and running, you can use this [docker](https://hub.docker.com/_/postgres) to set one up, as shown [below](https://github.com/VCityTeam/Spatial-Multimedia-DB-docker/blob/master/README.md#using-docker-compose).

This docker is used to create a [Spatial Multimedia database](https://github.com/VCityTeam/Spatial-Multimedia-DB). It is used to easily deploy a database and simultaneaously add an admin account. This database is used to store documents you can visualize in a 3D environment and interact with (follow the link above for more details).

## How to use it

### Using docker-compose (recommended if you do not have postgres yet or if you want an easier deployment)
<a name="using-docker-compose"></a>

- Copy the context folder provided in this directory in your compose folder.
- Create a .env file in your compose folder (if one already exists, do not create another)
- Add the environment variables located in [this file](Example/.env) to your own .env file.
- Configure the variables to suit your needs
- Build and launch your docker-compose (you can paste content from the one located in the [Example folder](./Example).
- Access your database with a dedicated software, or connect to the adress where it is deployed (usually http://localhost:<SPATIAL_MULTIMEDIA_DB_PORT>). You should see a webpage confirming that your deployment was successful.


### Known issue : Dockers not awaiting each other

Some dockers need to wait for another docker to be up and running so they can complete their task properly. In normal circumstances they do so. However, it has been observed on occasion that the SpatialMultimediaDB will start running while the postgres docker has not finished its setup, resulting in the SpatialMultimediaDB being unable to connect to the database. If a message similar to the one shown below shows up, relaunching the demo should solve the issue. 
   ```
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

### using docker (if you already have postgres installed)



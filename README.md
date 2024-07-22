# Docker Compose Node.js Application with MongoDB and Mongo-Express

This repository explains how to create a docker-compose file to deploy a Node.js application working along with MongoDB. We will also deploy Mongo-Express console to access the MongoDB database. All of this is deployed through a single  Docker Compose file. Finally, we will connect our Node.js application to the MongoDB database, fetch some values, and display them in the application.

## Prerequisites

Before you begin, ensure you have the following installed on your machine:
- [Docker](https://www.docker.com/products/docker-desktop)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Project Structure


## Getting Started
### Clone the Repository

```sh
git https://github.com/vishnuckm/docker-compose.git
cd docker-compose
```
The complete architecture of the repo will loock like this

### image here>>>>>>>>>>>>>

Here you can see a bunch of application files, Dockerfile and Docker compose file that you can use for this project.

The be low docker compose file will create "[MongoDB](https://hub.docker.com/_/mongo)" "[Mongo-Express](https://hub.docker.com/_/mongo-express)" and the "web-app"  which is needed for our project.


```
version: '3.1' #version of docker compose
services:
  my-app:
    build: .
    ports:
      - 3000:3000
    environment:
      - MONGO_DB_USERNAME=${MONGO_ADMIN_USER}
      - MONGO_DB_PWD=${MONGO_ADMIN_PASS}
  mongodb:
    image: mongo
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=${MONGO_ADMIN_USER}
      - MONGO_INITDB_ROOT_PASSWORD=${MONGO_ADMIN_PASS}
  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8081:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=${MONGO_ADMIN_USER}
      - ME_CONFIG_MONGODB_ADMINPASSWORD=${MONGO_ADMIN_PASS}
      - ME_CONFIG_MONGODB_SERVER=mongodb
    depends_on:
      - "mongodb"
```

Create ENVs for the mongodb username and password with the below command in your console.
```
export MONGO_ADMIN_USER=admin
export MONGO_ADMIN_PASS=password
```
Now deploy the docker-compose file using the below command.
```
docker-compose -f docker-compose.yaml up -d
```



### Verify the Setup 

Node.js Application: The application should be running on [http://localhost:3000](http://localhost:3000).

![image](https://github.com/user-attachments/assets/9abaa045-3811-4f12-b2ae-601c3508da82)

Mongo-Express: The Mongo-Express console should be accessible at [http://localhost:8081](http://localhost:8081). 
The default login username and password for mongoexpress is "admin" and "pass"

You might need to wait for a couple of minutes for the MongoExpress to be available int he browser (1-3 minutes).

![image](https://github.com/user-attachments/assets/516b7c30-2aa5-49cb-a0a9-b25187e12e3f)

Now as you can see the application is showing no value in the Data from MongoDB section to add data over that section you need to do the below steps.

Create a new database named "new-db"
![image](https://github.com/user-attachments/assets/2e02e12c-362d-47c6-87c1-ed1d2aa92f2c)

Create new collection inside that "Database".
![image](https://github.com/user-attachments/assets/65734a82-991d-4a7b-a8ee-dbcaed76156b)

Then add a "new-document" in the collection as below.
![image](https://github.com/user-attachments/assets/c772136b-18b0-4b07-b680-0b737094cbff)

If you changed the values of "database name", "collection name",  "id" and "data" you need to edit the application files inside the "app" directory for the application to properly work . 

Now again refresh the application webpage and see the updated content.
![image](https://github.com/user-attachments/assets/7775842f-9c8e-4038-85ad-d7b28259159b)

That's it Your application is ready to rock.


By following the steps outlined in this repository, you can easily containerize a Node.js application and set up a MongoDB instance with a web-based Mongo-Express interface. This setup ensures that your application can interact with the database and display data fetched from MongoDB.

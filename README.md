# docker-compose
This Repository expalins how to covert a nodejs application as a docker container and run it. Then we are going to create a mongodb and access it via mongo-express console which we will deploy through docker-compose file. Finally we will connect our database with the application and get some value fetched from the mongodb and load it in the application.    

# Docker Compose Node.js Application with MongoDB and Mongo-Express

This repository explains how to convert a Node.js application into a Docker container and run it. We will create a MongoDB instance and access it via the Mongo-Express console, all deployed through a Docker Compose file. Finally, we will connect our Node.js application to the MongoDB database, fetch some values, and display them in the application.

## Prerequisites

Before you begin, ensure you have the following installed on your machine:
- [Docker](https://www.docker.com/products/docker-desktop)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Project Structure
project-root/
│
├── Dockerfile
├── docker-compose.yaml
├── .env
└── app/
├── package.json
├── package-lock.json
└── server.js

## Getting Started
### 1. Clone the Repository

```sh
git clone https://github.com/your-repository.git
cd your-repository

### 2. Create a .env File
Create a .env file in the root directory of the project and add the following environment variables:

MONGO_ADMIN_USER=admin
MONGO_ADMIN_PASS=password
### 3. Build and Run the Containers
Use Docker Compose to build and start the containers:

docker-compose -f docker-compose.yaml up -d

### 4. Verify the Setup
Node.js Application: The application should be running on http://localhost:3000.
Mongo-Express: The Mongo-Express console should be accessible at http://localhost:8081.
### 5. Accessing Mongo-Express
You can log in to Mongo-Express using the credentials defined in the .env file:

Username: admin
Password: password
### 6. Connecting Node.js Application to MongoDB
The Node.js application is set up to connect to the MongoDB instance using the credentials provided in the .env file. Ensure your server.js file handles the MongoDB connection appropriately.

Dockerfile
The Dockerfile sets up the Node.js application in a Docker container:

Dockerfile

FROM node:20-alpine

RUN mkdir -p /home/app

COPY ./app /home/app

# Set default directory so that next commands execute in /home/app directory
WORKDIR /home/app

# Install dependencies
RUN npm install

# Run the application
CMD ["node", "server.js"]
Docker Compose Configuration
The docker-compose.yaml file defines the services for the Node.js application, MongoDB, and Mongo-Express:

version: '3.1'
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
Conclusion
By following the steps outlined in this repository, you can easily containerize a Node.js application and set up a MongoDB instance with a web-based Mongo-Express interface. This setup ensures that your application can interact with the database and display data fetched from MongoDB.
![image](https://github.com/user-attachments/assets/7775842f-9c8e-4038-85ad-d7b28259159b)

![image](https://github.com/user-attachments/assets/c772136b-18b0-4b07-b680-0b737094cbff)

![image](https://github.com/user-attachments/assets/9abaa045-3811-4f12-b2ae-601c3508da82)

![image](https://github.com/user-attachments/assets/516b7c30-2aa5-49cb-a0a9-b25187e12e3f)


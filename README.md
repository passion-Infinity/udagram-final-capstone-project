# Project Overall
This is a final project of udagram. It's the Capstone Project. I re-used code from my course 4 project. Build & deploy a serverless app, the simple TODO application using AWS Lambda and Serverless framework.

* It is built on Serverless technology using AWS Lambda.

* It is built on React library, ran docker locally and deployed on kubernetes cluster.

## GitHub Repository Link:
`https://github.com/passion-Infinity/udagram-final-capstone-project`

# Functionality of the application

This application will allow creating/removing/updating/fetching TODO items. Each TODO item can optionally have an attachment image. Each user only has access to TODO items that he/she has created.

## Backend

This project has some the following major functions and configure them in the `serverless.yml` file:

* `Auth` - this function should implement a custom authorizer for API Gateway that should be added to all other functions.

* `GetTodos` - should return all TODOs for a current user. A user id can be extracted from a JWT token that is sent by the frontend

* `CreateTodo` - should create a new TODO for a current user. A shape of data send by a client application to this function can be found in the `CreateTodoRequest.ts` file

* `UpdateTodo` - should update a TODO item created by a current user. A shape of data send by a client application to this function can be found in the `UpdateTodoRequest.ts` file

* `DeleteTodo` - should delete a TODO item created by a current user. Expects an id of a TODO item to remove.

It should return an empty body.

* `GenerateUploadUrl` - returns a pre-signed URL that can be used to upload an attachment file for a TODO item.

# Frontend

The `client` folder contains a web application that can use the API that should be developed in the project.

This frontend should work with your serverless application once it is developed, you don't need to make any changes to the code. The only file that you need to edit is the `config.ts` file in the `client` folder. This file configures your client application just as it was done in the course and contains an API endpoint and Auth0 configuration:

```ts
const apiId = '...' API Gateway id
export const apiEndpoint = `https://${apiId}.execute-api.us-east-1.amazonaws.com/dev`

export const authConfig = {
  domain: '...',    // Domain from Auth0
  clientId: '...',  // Client id from an Auth0 application
  callbackUrl: 'http://localhost:3000/callback'
}
```

## Authentication

To implement authentication in your application, you would have to create an Auth0 application and copy "domain" and "client id" to the `config.ts` file in the `client` folder. We recommend using asymmetrically encrypted JWT tokens.

## Deployment
Kubernetes cluster
Using eksctl to create kubernetes cluster

```
eksctl create cluster --name <cluster-name> --region <region-code> --nodegroup-name <nodegroup-name> --node-type <node-type>

ex: region -> us-east-1
ex: node-type -> m5.large
```

# How to run the application

## Backend

To deploy an application run the following commands:

```
cd backend
npm install
sls deploy -v
```

## Frontend

To run a client application first edit the `client/src/config.ts` file to set correct parameters. And then run the following commands:

### Locally system

```
cd client
npm install
npm run start
```

or

```
docker-compose -f client/ up -d
```

or

```
docker build -f client/Dockerfile -t <image-name> ./client
docker run -it --name <container-name> -p <port>:80 <image-name>
```

or

```
docker pull nlcd/udagram-todos-client
docker run -it --name <container-name> -p <port>:80 <image-name>
```

This should start a development server with the React application that will interact with the serverless TODO application.


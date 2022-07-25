# List of Commands

## To Remove All Stopped Containers

```bash
docker container prune
```

## To Stop Running Containers

```bash
docker stop <name_of_container>
```

## Run Mongo with Docker

The following command run Mongo with Docker with a custom name in detached mode with a custom port

```bash
docker run --name mongodb --rm -d -p 27017 mongo
```

## Build Backend Dockerfile

```bash
docker build -t goals-node .
```

## Run Backend Container

You need to change `mongoose connect` line from `localhost` to `host.docker.internal` since it is communicating between other containers.

Remember to publish `-p` the port or the frontend will not be able to connect to the backend.

```bash
docker run --name goals-backend -d --rm -p 80:80 goals-node
```

## Build Frontend Dockerfile

```bash
docker build -t goals-react .
```

## Run Frontend Container

1. Remember to publish `-p` the port or the frontend will not be able to connect to the backend.
2. Because you want to interact with the react app, you must add `-it` when running it to tell Docker that you are using the app interactively.

```bash
docker run --name goals-frontend -d --rm -p 3000:3000 -it goals-react
```

# Adding Docker Network for Cross-Container Communcation

Check Available Docker Networks

```bash
docker network ls
```

# Create Docker Network

```bash
docker network create goals-network
```

# Running Containers Within Same Network

You can now run the following containers in the same network without the need of publishing the ports.

## runs mongodb within the same network

```bash
docker run --name mongodb --rm -d --network goals-network mongo
```

## runs backend within the same network

1. You must change the `mongoose connect` line in `app.js` from `host.docker.internal` to `mongodb` since it is using the same network.
2. Rebuild the container with:

```bash
docker build -t goals-node .
```

**Finally**:

```bash
docker run --name goals-backend -d --rm -p 3000:3000 --network goals-network goals-node
```

## runs frontend within the same network

1. As for **frontend**, you do not to modify `localhost` to `goals-backend` because it is the only way for React to communicate. Instead, you need to publish `-p` the backend container with the port 80 with command above.

```bash
docker run --name goals-frontend -d --rm ---network goals-network -p 3000:3000  -it goals-react
```

# Adding Data Persistance to MongoDB with Volumes

The goal of adding volumes is to make sure that data created is not taken down when container is stopped.

1. By adding `-v data:/data/db` it allows Docker to store data in the host machine.
2. Adding `-e MONGO_INITDB_ROOT_USERNAME=sofian -e MONGO_INITDB_ROOT_PASSWORD=password` allows authentication, you have to modify the **backend** (mongoose connect line) `mongodb://mongodb:27017/course-goals` from to `mongodb://sofian:password@mongodb:27017/course-goals`

```bash
docker run --name mongodb -v data:/data/db --rm -d --network goals-network -e MONGO_INITDB_ROOT_USERNAME=sofian -e MONGO_INITDB_ROOT_PASSWORD=password mongo
```

# Live Source Code Updates For React Container

1. You don't need nodemon with React since it will update the file as it changes natively.
   **NOTE: WINDOWS users will need to configure the Linux file system. REFER to `docs` folder in this repo.**

Adding `-v <relative_path_of_frontend_app.js>` binds the image to the source folder.

```bash
docker run -v <relative_path_of_frontend_app.js> --name goals-frontend --rm -p 3000:3000 -it goals-react
```

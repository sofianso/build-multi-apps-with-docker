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
# List of Commands

## To Remove All Stopped Containers
```bash
docker container prune
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
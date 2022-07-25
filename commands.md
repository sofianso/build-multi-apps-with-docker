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

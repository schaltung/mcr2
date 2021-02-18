# About using the Docker container

``
cd mcr2/docker 
TAG=$(date '+%Y-%m-%d')
mkdir -p ../runtime/$TAG/saved_models
RUNTIME_DIR_PATH=$(realpath ../runtime/$TAG)
``



## How to build

```
docker build \
  --no-cache=true \
  --build-arg USER=$(id -u -n) \
  --build-arg GROUP=$(id -g -n) \
  --build-arg UID=$(id -u) \
  --build-arg GID=$(id -g) \
  --tag mcr2:$TAG .
```


## How to provision (run) a container

```
CONTAINER_NAME=lab_$(date '+%s')
docker run \
   --gpus=all \
   -v $RUNTIME_DIR_PATH:/mnt/runtime \
   -t -d  \
   --name $CONTAINER_NAME \
   mcr2:$TAG
```

## How to enter (and use) the container
```
CONTAINER_ID=$(docker inspect $CONTAINER_NAME | jq -r .[0].Id)

docker exec -it $CONTAINER_ID bash
```

### ***Creating and Managing volumes***

* Creates named volume `docker volume create my-vol`
* Anonymous volumes `docker volume create`
* Lists the volumes present `docker volume ls`
* used for inspecting the volume `docker volume inspect my-vol`
* Remove the volume `docker volume rm my-vol`

## ***Docker volumes***
Prefered mechanism to persist the data. Completely managed by the docker only. \
stores files in the `/dev/vda*` mostly

*Example on using -mount flag*
```
docker run \
   --name volume_2 \
   -p 8090:80 \
   --mount type=volume,source=my-vol,target=/myapps \
   -d \
   nginx:alpine-slim
```
*Example on using -v flag*
```
docker run --name volume_2 -p 8090:80 -v my-vol:/myapps -d nginx:alpine-slim 
```
if my-vol is name of the docker volume to be used, if this is not present \
then Docker will create. 

***Verify and Inspect volume mount***
* Login to container
>  docker exec -it volume_2 /bin/sh
* verify disk space
>  df -h
/dev/vda1 drivers will be mounted to the /myapps dir means that the volume is mapped sucessfully.


## ***Guess ???***
### *1. Non-empty target dir*

```
 ls /usr/share/nginx/html
  index_1.html
  index_2.html

docker run \
   --name volume_2 \
   -p 8090:80 \
   --mount type=volume,source=my-vol,target=/usr/share/nginx/html \
   -d \
   nginx:alpine-slim
```
> No Data loss

> Old static content is automatically copied to the specified volume.

### *2. Two containers using same volume ?*

`myvol3 is already mapped with volume_demo1 container. What if we again map it to the volume_demo2 container with same mount path ?`
 > No error volume gets mounted successfully without any data loss.


### ***How to mount a volume as readonly***

1. readonly using --mount flag
```
docker run \
  --name readonly_volume \
  -p 8090:80 \
  --mount type=volume,source=myvol103,target=/usr/share/nginx/html,readonly \
  -d \
  nginx:alpine-slim
```

2. readonly using -v flag
```
docker run \
  --name readonly_volume \
  -p 8090:80 \
  -v myvol103:/usr/share/nginx/html:ro \
  -d \
  nginx:alpine-slim
```

### ***mounting a sub-directory as the volume***
* Granular control over the volumes.
* Useful for sharing specific data.

Example: 
Here as part of this demo we are using the myvol103 and assigning only the apps/ 
myvol103
  |- index.html
  |- read.sh
  |- file1.txt
  |-apps/   
```
docker run \
  --name volume_sub-dir \
  -p 8090:80 \
  --mount type=volume,source=myvol103,destination=/usr/share/nginx/html,volume-subpath=/apps \
  -d \
  nginx:alpine-slim
``` 
this above example is as same as below,
```
docker run \
  --name volume_sub-dir \
  -p 8090:80 \
  --mount type=volume,source=myvol103/apps,destination=/usr/share/nginx/html\
  -d \
  nginx:alpine-slim
``` 
volume-subpath concept is mainly used in the Docker compose.


## ***Docker Bind mounts***

* These are completely dependent on the directory structure.
* Files or directories on the host machine gets mounted into the container.
* Files/dir are referenced by thir absloute path on the host machine. And they need not exist already.

syntax:
```
docker run --mount type=bind,source=<absloute_path>,target
```
Example 
```
docker run \
   --name bind_mount_1 \
   -p 8090:80 \
   --mount type=bind,source="$(pwd)"/static-content,target=/usr/share/nginx/html,readonly \
   -d \
   nginx:alpine-slim
```
```
docker run \
   --name bind_mount_1 \
   -p 8090:80 \
   -v "$(pwd)"/static-content:/usr/share/nginx/html:ro \
   -d \
   nginx:alpine-slim
```
* Addition/modification of the files inside the container will reflect on the host machine && vice-versa.

*Verify the mounts*
```
docker ps
docker ps --format "table{{.Image}}\t{{.Names}}\t{{.Status}}\t{{.ID}}\t{{.Ports}}"     #formating the output of the docker ps 
docker inspect bind_mount_1
docker inspect --format=`{{json .Mounts}} bind_mount_1 | jq
```

*target as Non-empty dir scenario using bind mounts*
```
docker run --name=mynginx -p 8092:80 -d nginx:alpine-slim
```
> docker exec -it mynginx /bin/sh
> cd /usr/share/nginx/html
> index.html and other files are present by default

*Attaching the volume mount to the /usr/share/nginx/html*
```
docker run --name=nonempty_vol -p 8091:80 --mount type=volume,src=myvol1,dst=/usr/share/nginx/html,readonly -d nginx:alpine-slim
```
> docker exec -it nonempty_vol /bin/sh \
> df -h \
> /usr/share/nginx/html is mounted as the volume /dev/vda* \
> no data loss

*Attaching the bind mount to the /usr/share/nginx/html*

```
docker run --name=nonempty_vol2 -p 8091:80 --mount type=bind,src=/static-content,dst=/usr/share/nginx/html,readonly -d nginx:alpine-slim
```
> docker exec -it nonempty_vol2 /bin/sh \
> df -h \
> /usr/share/nginx/html is mounted as the overlay \
> and the old files will be replaced with the files present in the static-content \
> Data loss 



   



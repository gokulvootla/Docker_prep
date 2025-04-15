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
  

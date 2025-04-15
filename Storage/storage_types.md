## *Need of Storage in Docker*
* Docker containers are designed to be ephemeral, meaning that any data created or modified within a container is lost when the container is deleted.
* Accessing data out of the container if required by other processes is difficult.

To Persist the data irrespective of the container's lifecycle. Docker Provides,

>  Docker Volumes \
>  Bind mounts

![Docker_storage](https://miro.medium.com/v2/resize:fit:746/1*bViAujGNZjJQrmhfN6IRsQ.png)

* These storage types can be used by adding either of the flags like,
> --mount flag \
> --voulme

### ***Using `--mount`***
In general, --mount flag is preffered. Explicit way of mounting and supports all the available options. \ 
Consists of multiple key-value pairs. Better for complex scenarious. \
Options:
```
source/src                --->  For named volumes mention volume name ;  anonymous volumes, this field is omitted.
target/destination/dst    --->  Path where files or directory is mounted on container.
volume-subpath            --->  Subdirectory within the volume to mount into the container.
ro/readonly               --->  Volume to be mounted as readonly.
volume-nocopy             --->  Data at the destination isn't copied into the volume if the volume is empty
```
Syntax
```
--mount type=volume,source=<voulme_name>,destination=<container's_target_path>
```
Example (--mount usage with bind mounts)
```
docker run --name volume_1 \
  -p 8090:90 \
  --mount type=bind,source=/test/index,target=/mapps \
  -d \
  nginx:alpine-slim
```
Example (--mount usage with volume mounts)
```
Multi-line:
---------------------------------------------------------------------------
docker run --name volumes_1 \
    -p 8090:80 \
    --mount type=volume,source=myvol1,target=/myapps \
    -d \
    nginx:alpine-slime

single-line:
---------------------------------------------------------------------------
docker run --mount type=volume,src=myvolume,dst=/data,ro,volume-subpath=/foo
```
### ***Using `--volume`***
For quick volume mounts. Can be declared using -v or --volume. \
less supported options.

syntax: 
```
 docker run -v [<volume-name>:]<mount-path>[:opts]
```
Example 
```
docker run \
  --name volume_2 \
  - p 8090:80 \
  -v "$(pwd)"/static-content:/usr/share/nginx/html \
  -d \
  nginx:alpine-slim
```


### *Docker tmpfs mounts:*

 Docker volumes & Bind-mounts allows to share the files between container and host machine.So, Data persists
 even if the container stops. \
 Here tmpfs(temporary file system) mounts resides on the host system's memory. \
 When the container stops, the tmpfs mount is removed, and files written there won't be persisted.
 
syntax
```
docker run --mount type=tmpfs,dst=<mount-path>
```
Example
```
docker run --name tmpfs_1 \
  --mount type=tmpfs,destination=/app -d \
  nginx:alpine-slim
```

Limitations:
1) Ephemeral storage.
2) No sharing of data between the containers.
3) Memory consumption (tmpfs consumes RAM, excessive use can exhaust) 












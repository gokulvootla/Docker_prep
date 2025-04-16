## ***tmpfs mounts***

* Docker volumes & bind-mounts allows to share files between the host-machine and the container. So, Data persists even if the container stops.
* tmpfs mounts resides on the host system's memory.

syntax: 
> --mount type=tmpfs,destination=<>

Example: 
> docker run --name tmpfs_example --mount type=tmpfs,destination=/apps -d nginx:alpine-slim

* docker inspect tmpfs_example --format `{{json .Mounts}}` 
* docker exec -it tmpfs_example /bin/sh 
* /apps of the container data is stored under the tmpfs of the host machine 
* once the container stops & starts, data vanish since tmpfs uses the RAM of the host machine.

1. RAM size allocation to the tmpfs mounts ?
   > tmpfs max have 50% of the hosts total RAM.
   > customization is possible using the `--tmpfs-size`
   ```
   docker run --name tmpfs_example1 --mount type=tmpfs,destination=/app,tmpfs-size=100m -d nginx:alpine-slim
   ```
    Inspecting the size of RAM that is attached,
      > docker exec -it tmpfs_example1 bin/sh
      > df -h
      > tmpfs 100.0M 0 100.m /app

Limitations of tmpfs:
* Ephemeral storage
* No data sharing between the containers.
* tmpfs consumes RAM, excessive usage can exhaust the host.
   


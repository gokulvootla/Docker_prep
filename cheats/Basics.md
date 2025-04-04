
# **Docker Usefull Commands** 


##  *Basic Commands*
- docker pull nginx:v1 ***(pull the image)***

- docker run --name myapp1 -p 8080:80 -d nginx:v1 ***(running container out of image)***

- docker exec -it myapp1 /bin/bash ***(connecting to the container)***

- docker ps ***(list running containers)***

- docker ps -a ***(list all the containers)***

- docker images ***(list all the images)***

*docker logging is required to push the image & pull from the private repositories*

- docker login -u  <_username_> ***(prompts for password)***

- docker login ***(web based auth)***

- docker logout


***Executing commands directly on the running containers...***
1.  docker exec -it myapp1 ls
2.  docker exec -it myapp1 date
3.  docker exec -it myapp1 printenv
4.  docker exec -it myapp1 hostname
5.  docker exec -it myapp1 df -h
## *Remove Container*
 1. docker rm myapp1 *(stopped container)* 
 2. docker rm myapp1 -f *(running container)* 
 3. docker ps -qa | xargs docker rm  *(remove all containers)* 
## *Remove Images*
 1. docker rmi nginx:v1 
 2.  docker images -q | xargs docker rmi *(remove all the images)*

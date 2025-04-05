
#  *Dockerfile*
It is a textfile contains the set of instructions to build a docker
image

***why Dockerfiles* ?**
 - Used to install software.
 - copy files.  
 - set env.
 - run commands.
 - define Entry Point for the application.

***Example Dockerfile***

    FROM nginx:alpine-slim 
    COPY index.html /usr/share/nginx/html

 - docker build -t custom-nginx:v1 .  ***(build docker image)***
 
 - docker run --name mynginx -p 8090:80 custom-nginx:v1 ***(running   
   image as container)***
  
 - docker tag custom-nginx:v1   
   `<docker_hub_profile_name>`/custom-nginx:v1 ***(tag the image with   
   docker hub-id)***
   
 - docker push`<docker_hub_profile_name>`/custom-nginx:v1 ***(docker   
   push)***

***SEARCH FILTERS*** 
 - docker search nginx 
 - docker search nginx --limit 5 ***(shows only 5
   images of nginx)*** 
 - docker search nginx --filter=star=50 ***(gives    the images which
   contains 50 stars )*** 
 - docker search nginx       --filter=is-official=true ***(gives only
   the official images of the nginx)**


***LABELS***
-
There are 2 types of the labels 
a. Maintainer 
b. OCI ***(open containers initiative)***

maintainer labels got deprecated but still no-harm to use...

     - LABEL maintainer=""
     - LABEL version="1.0"
     - LABEL description=""

OCI LABELS:

     - LABEL org.opencontainers.image.autho="GOKU VOOTLA" 
     - LABEL org.opencontainers.image.title="" 
     - LABEL org.opencontainers.image.description="" 
     - LABEL org.opencontainers.image.vendor=""  
     - LABEL org.opencontainers.image.revision=""  
     - LABEL org.opencontainers.image.created=""  
     - LABEL org.opencontainers.image.url=""  
     - LABEL org.opencontainers.image.source=""`<git_hub>`  
     - LABEL org.opencontainers.image.documentation=""  
     - LABEL org.opencontainers.image.licenses=""  
     - LABEL org.opencontainers.image.version=""

## ***DOCKER INSPECT***

docker image inspect `<image-name/id>` ***(inspect the docker image)***
 docker image inspect `<image-name/id>` --format='{{json.Config.Labels}}' ***(gives exactly the labeled content)***
 

>  ***install the jq tool to see in a json***
>   docker image inspect`<image_id>` --format='{{json.Config.Labels}}' \| jq 

1. docker inspect
`<container_id>` 
2. docker inspect `<container_id>`--filter='{{json .NetworkSettings}}' 
3. docker inspect
`<container_id>`{=html} --filter='{{json.Config.ExposedPorts}}' 
4. docker inspect `<container_id>`--filter='{{json.NetworkSettings.IPAddress}}"

## ***COPY***

Used to copy files or directories from the source and add them to the
filesystem of the images at mentioned path
files and directories can be copied from..

    a. build context
    b. build stage
    c. named context
    d. an image


## ***ADD***

Used to copy files from the source to images destination
files and directories can be copied from.. 

    a. build context 
    b. remote URL 
    c. a git repo


***COPY*** ***Vs*** ***ADD***
--
>       | File Transfer |   Build context     | Build context
>       | Tar archive   |      NO             |  YES
>       | URL support   |      NO             |  YES
>       | Performance   |     FAST            | slower in  advanced option
>       | Security      |safer for local copy |  Risk in URL Fetch** 
>       | USE Case      | Local Copy          | URL & Tar Copy

## ***ARG***
Used to define the build-time variables. In the final image these variables are not present.
* We can use 1 (or) more ARG instructions.
* ENV overrides the ARG.
* Can be overridden at build stage by passing <^>--build-arg<^>

  __***syntax***__
docker build --build-arg<variable_name>= <value>
### Examples
> ARG NGINX_VERSION=1.26 \
FROM nginx:${NGINX_VERSION}-alpine-slim \
LABEL org.opencontainers.image.author="Gokul Vootla"\
LABEL org.opencontainers.image.version="1.0" \
COPY index.html /usr/share/nginx/html

***Building the above Dockerfile***

     docker build -t args_demo . 

***Nginx Version***

    docker exec -it args_demo nginx -v

***Overriding ARG***

    docker build --build-arg NGINX_VERSION=1.27.0 -t overriden_args:v1 .

## ***ENV***
Sets Environmental variables && can be accessible at build as well as run times.
Helps to configure container's environment. ENV's persists in the final image. 
Can influence the behavior of running container.

 __***syntax***__
ENV NAME="Gokul" 
ENV SCHOOL=ABC\ Public\ school

* *Insingle line we can also declare,*
   > ENV NAME="Gokul" SCHOOL=ABC\ Public \school



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

* single line declaration,
   > ENV NAME="Gokul" SCHOOL=ABC\ Public \school
   
Overriding the ENV from docker run command..
```
FROM ubuntu
ENV NAME="GOKUL"
CMD echo $NAME
```
```
docker build -t myenv .
```
```
docker run --name envdemo -e NAME="Sarala" myenv
#output
Sarala
```

### ***ARG vs ENV***
        |   Feature     |      ARG             |      ENV               |
        |    Scope      |   Build-time         |    Runtime             |
        |  Persistence  | Not in final image   | Persist in final image |
        |   Usage       |  Setting variables   | Setting environmental  | 
                           durning Build-time        variables     
        |    syntax     |ARG <name>=[<default>]|   ENV <key>=<value>    | 

### ***ARG and ENV Combined usage***
* ARG instructions allows to pass variables at build time without modifying the Dockerfile. So, by combining both ENV && ARG we can pass values configured in build process into the runtime environment.
   > ARG (Build-time customization)
   
   > ENV (Runtime customization)
 * This separation makes Dockerfile flexible at both build and run stages without Overlapping.
 * On Using ENV, we can setup the sensitive information such as API keys or database credentials 
 * This combination creates Docker image more reusable and adaptable to various deployment scenarios. 

***Example To  Show combined usage of ENV && ARG***
 > FROM python:3.12-alpine \
     LABEL org.opencontainers.image.author="Vootla" \
     LABEL org.opencontainers.image.version="1.2"  \
     LABEL org.opencontainers.image.title="Combined usage of ENV and ARG" \
     ARG ENVIRONMENT=dev \
     ENV APP_ENVIRONMENT=${ENVIRONMENT}
     
***Initially***
```
ARG && ENV = dev
```
***Overriding ENV from ARG***
```
docker build --build-arg ENVIRONMENT=QA -t arg_env .

docker run --name myenv1 -p 8080:80 -d demo-arg:v1 

~~Verifying~~

 docker exec -it myenv1  env|grep APP_ENVIRONMENT   ###APP_ENVIRONMENT=QA 
```
*   At Build-time passed parameters gets overridden.
*   ENV is also parameterized, so it gets the value of ARG
Then ARG=ENV=QA

***Overriding from ENV***

```
docker run --name demo2 -e APP_ENVIRONMENT=dev -p 8070:80 -d demo-arg-env:v1 

~~Verifying~~

 docker exec -it myenv1  env|grep APP_ENVIRONMENT   ###APP_ENVIRONMENT=dev
```
## ***WORKDIR***

Defines working directory for instructions\
RUN, CMD, ENTRYPOINT, COPY & ADD.

* Example
  > WORKDIR /app \
  > COPY requirements.txt requirements.txt \
  
  * Meaning it copies requirement.txt locally to /app/requirements.txt
 * Using Base image 
✅, WORKDIR likely to be set by base image itself.
***Default WORKDIR "/"***

```
🤔🤔🤔🤔
what if Dockerfile contains something like ?

WORKDIR /opt
WORKDIR apps
WORKDIR myapp1
RUN PWD

Output: 
    /opt/apps/myapp1 
```

## Executing Commands
CMD, ENTRYPOINT & RUN are used to execute the commands from the Dockerfile. Methods for defining commands,

***1. Exec form***
>Command gets executed directly, written in JSON array format each element in the array represents an argument to the command.
```
 RUN ["pwd"]
 CMD ["sleep","ls"]
 EMTRYPOINT ["node","app.js"]
 RUN ["apt-get", "update"] 
 RUN ["apt-get", "install","-y","pthon3"]
```

**Pros:**
1. Portable as the commands get executed directly
2. Secure as it avoids shell injection vulnerabilities
3. Clear for complex commands as each argument is clearly.

**Cons**
1. Hard to read for simple commands.
2. Lack of shell features.

***2. Shell form***
> Can be written as a single string, executed in container's shell (/bin/sh -c) 
```
 RUN echo $HOME
 CMD echo $PATH
 ENTRYPOINT node app.js
 RUN apt-get update && apt-get install -y pthon2
```
**Pros**
1. Easy to write and read
2. Can use shell features like env fetching, chaining commands with (&&)

**Cons**
1. Hard to manage complex commands
2. less portable as it relies on shell
-----------------
### *Usage*:

> **Shell form** is best if shell features are needed.\
> **Exec form** best for complex commands and security.
------------------------
### *containers are designed for running Tasks and process. Task completed! container stops.*
>  In Dockerfile ***CMD and ENTRYPOINT*** Specifies the process running in the container.

## ***CMD***
* Used to set default command that is to be executed when container starts.
* Users can Override CMD easily by providing command while running the container.
* ***Only 1 CMD/Dockerfile, If there are multiple CMD in a Dockerfile then last one will be executed.***

```
FROM ubuntu:20.04 
CMD ["echo","Hello World!"]  #exec form 
CMD echo "Hello World!"    #shell form 
```
Example:
```
CMD ["nginx" , "-g" , "daemon off"]

docker run --name mycmd -it myimage  --> this will runs the nginx image
```
Guess???
```
docker run --name mycmd -it myimage /bin/sh 
```
Specifing the /bin/sh while running the container will overrides the CMD in Dockerfile \
In this example it will not run the nginx image.


## ***ENTRYPOINT***
Used to define a container with a specific executable that runs always. Can be overridden by providing --entrypoint flag.

```
FROM ubuntu:20.04
ENTRYPOINT ["echo","Hello"]   #exec form
ENTRYPOINT echo "hello"       #shell form
```
Example:
```
FROM ubuntu:20.04
ENTRYPOINT["echo","Hello World!"]
```
```
docker run myimage ofJoy
```
```
ofJoy will be appened to the ENTRYPOINT instuction

echo "Hello World" ofJoy --> ENTRYPOINT will run this command 
Hello World ofJoy #output

Here the arguments that we pass will gets appended to the ENTRYPOINT instruction,

docker run myimage env 

Hello World env #output
```
## ***Combined usage of ENTRYPOINT and CMD***
* We can use CMD and ENTRYPOINT together.
* CMD provides default values to the ENTRPOINT
* ENTRYPOINT (Specifies the executable ) + 
CMD (default arguments) 
* While using ENTRYPOINT and CMD together then make sure to keep them in exec form.

*Example1*
```
ENTRYPOINT ["python", "app.py"]
CMD ["--help"]
```
By default docker run will execute 
 > pthon app.py --help

docker run <image> --version will execute 
> python app.py --version

*Example2*
```
FROM ubuntu:20.04
ENTRYPOINT ["echo","Hello"]
CMD ["World"]

>) Running the container without any argument 
echo "Hello World!"

>) docker run myimage vootla
echo "Hello vootla"

Because CMD's World argument gets overriden by vootla... 
```
## ***NOTE***
```
when command run via ENTRYPOINT followed by CMD Then docker automatically assumes that value passed to the CMD is any
argument ; not a command to be overridden.
```




## ***RUN***

Used to execute the commands durning the build process of the docker image. \
Each run command creates the layer in the Docker image.Mostly used for installing the software packages. \

**Recomendations for RUN**
* Minimizing Layers
     Combine multiple RUN commands into a single Instructions by using the chaining mechanism using the (&&)       
          `RUN apt-get update && apt-get install -y nginx`
* Cache
     Cache isn't invalidated automatically durning the next build so --no-cache flag helps alot \
                 `RUN apt-get --no-cache dist-upgrade -y`

To install tools 
```
FROM ubuntu:latest
MAINTAINER gokul@gmail.com
RUN apt-get update
RUN apt-get install -y apache2
```
To run a script
```
COPY test.sh .
RUN ./test.sh
#OR
RUN /path/to/test.sh
```


## ***EXPOSE***

Specifies the port that container will listens on at runtime. \
Default protocol is tcp.
syntax
 > EXPOSE port/<protocol>
Examples
 > EXPOSE 8080/udp \
 >  EXPOSE 7000-8000 \
 > Expose 8080 8081 8082
```
FROM ubuntu:latest
EXPOSE 80/tcp
EXPOSE 80/udp
CMD ["sleep", "300"]
```

### publishing the port ? 
EXPOSE will only documents the port, publishing ports will make accessible from the host machine. \
for that `-p, -P` flags comes into picture.



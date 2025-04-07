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

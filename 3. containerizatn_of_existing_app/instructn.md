More on Ports
Ports are not usually just randomly assigned numbers. Many ports are used for specific activities in networking. Port 80 is the commonly used port number for HTTP requests, which is why we are exposing that port to the host computer - it's where the Flask app is listening for requests. You may also see other ports, such as port 8080, used with different applications. What this means is that the server is listening on that port, so a request to that server must also append the "special" port number to an HTTP request (such as example.website.com:8080), so that the sending client uses port 8080, instead of the normal HTTP port 80.

https://utilizewindows.com/list-of-common-network-port-numbers/

https://runnable.com/docker/binding-docker-ports

Q: Match up the below network services to the correct port numbers. Not all options are used.

SERVICE                 PORT NUMBER
SSH                     22             

DNS                     53

HTTP                    80

POP3                    110

-: Common Issues Running a Container
There are a few common issues that crop up when starting a container or building one for the first time. Let's walk through each problem and then present a solution for them.

What Goes in a Dockerfile if You Need to Write to the Host Filesystem?
https://docs.docker.com/storage/volumes/

In the following example the docker volume command is used to create a volume and then later it is mounted to the container.

>  /tmp docker volume create docker-data
docker-data

>  /tmp docker volume ls
DRIVER              VOLUME NAME
local               docker-data

>  /tmp docker run -d \
  --name devtest \
  --mount source=docker-data,target=/app \
  ubuntu:latest
6cef681d9d3b06788d0f461665919b3bf2d32e6c6cc62e2dbab02b05e77769f4

How Do You Configure Logging for a Docker Container?
https://docs.docker.com/config/containers/logging/configure/

You can configure logging for a Docker container by selecting the type of log driver, in this example json-file, and whether it is blocking or non-blocking. This example shows a configuration that uses json-file and mode=non-blocking for an Ubuntu container. The non-blocking mode ensures that the application won't fail in a non-deterministic manner. Make sure to read the Docker logging guide on different logging options.

>  /tmp docker run -it --log-driver json-file --log-opt mode=non-blocking ubuntu 
root@551f89012f30:/#

How do You Map Ports to the External Host?
The Docker container has an internal set of ports that must be exposed to the host and mapped. 
https://docs.docker.com/engine/reference/commandline/port/
One of the easiest ways to see what ports are exposed to the host is by running the docker port <container name> command. Here is an example of what that looks like against a foo named container.

$ docker port foo
7000/tcp -> 0.0.0.0:2000
9000/tcp -> 0.0.0.0:3000

What about actually mapping the ports? You can do that using the -p flag as shown. You can read more about Docker run flags here.
docker run -p 127.0.0.1:80:9999/tcp ubuntu bash

https://docs.docker.com/engine/reference/commandline/run/

What about Configuring Memory, CPU and GPU?
You can configure docker run to accept flags for setting Memory, CPU and GPU. You can read more about it here in the official documentation. Here is a brief example of setting the CPU.
https://docs.docker.com/config/containers/resource_constraints/

docker run -it --cpus=".25" ubuntu /bin/bash

This tells this container to use at max only 25% of the CPU every second.
https://docs.docker.com/config/containers/resource_constraints/

Q: What can go wrong with an application starting in a container?
d container may need to write to d file sys
logging may need to be configured
a container may need to have CPU, Disk and Memory requiremts configured
it may need ports exposed internal, externally and in a cluster





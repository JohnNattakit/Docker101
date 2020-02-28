# Creating and Using Containers Like a Boss

## Check Our Docker Install and Config
verified cli can talk to engine
> docker version

most config values of engine
> docker info

docker

docker container run

docker run

## Starting a Nginx Web Server

docker container run --publish 80:80 nginx

docker container run --publish 80:80 --detach nginx

docker container ls

docker container stop 690

docker container ls

docker container ls -a

docker container run --publish 80:80 --detach --name webhost nginx

docker container ls -a

docker container logs webhost

docker container top

docker container top webhost

docker container --help

docker container ls -a

docker container rm 63f 690 ode

docker container ls

docker container rm -f 63f

docker container ls -a

## Container VS. VM: It's Just a Process

docker run --name mongo -d mongo

docker ps

docker top mongo

docker stop mongo

docker ps

docker top mongo

docker start mongo

docker ps

docker top mongo

## Assignment Answers: Manage Multiple Containers

docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes MYSQL_RANDOM_ROOT_PASSWORD

docker container logs db

docker container run -d --name webserver -p 8080:80 httpd

docker ps

docker container run -d --name proxy -p 80:80 nginx

docker ps

docker container ls

docker container stop TAB COMPLETION

docker ps -a

docker container ls -a

docker container rm

docker ps -a

docker image ls

## What's Going On In Containers: CLI Process Monitoring

docker container run -d --name nginx nginx

docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql

docker container ls

docker container top mysql

docker container top nginx

docker container inspect mysql

docker container stats --help

docker container stats

docker container ls

## Getting a Shell Inside Containers: No Need for SSH

docker container run -help

docker container run -it --name proxy nginx bash

docker container ls

docker container ls -a

docker container run -it --name ubuntu ubuntu

docker container ls

docker container ls -a

docker container start --help

docker container start -ai ubuntu

docker container exec --help

docker container exec -it mysql bash

docker container ls

docker pull alpine

docker image ls

docker container run -it alpine bash

docker container run -it alpine sh

## Docker Networks: Concepts for Private and Public Comms in Containers

docker container run -p 80:80 --name webhost -d nginx

docker container port webhost

docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost

## Docker Networks: CLI Management of Virtual Networks

docker network ls

docker network inspect bridge

docker network ls

docker network create my_app_net

docker network ls

docker network create --help

docker container run -d --name new_nginx --network my_app_net nginx

docker network inspect my_app_net

docker network --help

docker network connect

docker container inspect TAB COMPLETION

docker container disconnect TAB COMPLETION

docker container inspect

## Docker Networks: DNS and How Containers Find Each Other

docker container ls

docker network inspect TAB COMPLETION

docker container run -d --name my_nginx --network my_app_net nginx

docker container inspect TAB COMPLETION

docker container exec -it my_nginx ping new_nginx

docker container exec -it new_nginx ping my_nginx

docker network ls

docker container create --help

## Assignment Answers: Using Containers for CLI Testing

docker container run --rm -it centos:7 bash

docker ps -a

docker container run --rm -it ubuntu:14.04 bash

docker ps -a

## Assignment Answers: DNS Round Robin Testing

docker network create dude

docker container run -d --net dude --net-alias search elasticsearch:2

docker container ls

docker container run --rm -- net dude alpine nslookup search

docker container run --rm --net dude centos curl -s search:9200

docker container ls

docker container rm -f TAB COMPLETION


# What's In This Section
# 1. Check versions of our docker cli and engine
``` 
docker <command> <sub-command> (options)
Ex. docker container run 
    docker run //run forever
```
> sudo docker version

> sudo docker info


# 2. Create a Nginx (web server) container

# image vs. container
1. An Image is the application we want to run
2. A Container is an instance of that image running as a process
3. You can have many containers running off the same image
4. In this lecture our image will be the Nginx web server
5. Docker's default image "registry" is called Docker Hub
(hub.docker.com)

> docker container run --publish 80:80 nginx
1. Downloaded image 'nginx' from Docker Hub
2. Started a new container from that image
3. Opened port 80 on the host IP
4. Routes that traffic to the container IP, port 80

# Run docker into background
> sudo docker container run --publish 80:80 --detach nginx

# Run docker with specific name
> sudo docker container run --publish 80:80 --detach --name webhost nginx 

# Access Logs docker
> docker container logs <name>

# Docker Container Process
> docker container top webhost

# Docker Container list
> sudo docker container ls
> 
> docker container stop <contaner_id>
> 
> docker container ls -a

# Remove Docker Container
> docker container rm 9e5 395 b10
> 
> docker container rm -f b10


# What happens in 'docker container run'
1. Looks for that image locally in image cache, doesn't find
anything
2. Then looks in remote image repository (defaults to Docker Hub)
3. Downloads the latest version (nginx:latest by default)
4. Creates new container based on that image and prepares to
start
5. Gives it a virtual IP on a private network inside docker engine
6. Opens up port 80 on host and forwards to port 80 in container
7. Starts container by using the CMD in the image Dockerfile

> docker container run --publish 8080:80 --name webhost -d nginx:1.11 nginx
-T

# Container VS VM: it's just a process
> sudo docker run --name mongo -d mongo
> 
> sudo docker top mongo
> 
> ps aux |grep mongo

> sudo docker stop mongo 
>
> sudo docker top mongo
> 
> ps aux |grep mongo

> sudo docker start mongo
> 
> docker ps
> 
> docker top mongo

# Assignment: Manage Multiple Containers
* docs.docker.com and --help are your friend
* Run a nginx, a mysql, and a httpd (apache) server
* Run all of them --detach (or -d), name them with --name
* nginx should listen on 80:80, httpd on 8080:80, mysql on 3306:3306
* When running mysql, use the --env option (or -e) to pass in
MYSQL_RANDOM_ROOT_PASSWORD=yes
* Use docker container logs on mysql to find the random password
it created on startup
* Clean it all up with docker container stop and docker
container rm (both can accept multiple names or ID's)
* Use docker container ls to ensure everything is correct before
and after cleanup

# Answer assignment
# Create Docker Container MySQL Database
> sudo docker container run -detach -p 3306:3306 --name db --env MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
>
> docker container logs db
> 
> 2020-02-27 08:29:55+00:00 [Note] [Entrypoint]: GENERATED ROOT PASSWORD: aizaemei7aChaichahpoph7xohb5akah


# Create Docker Container HTTPD Webserver
> docker container run --detach -p 8080:80 --name webserver httpd

# Create Docker Container Nginx Proxy
> docker container run --detach --name proxy -p 80:80 nginx

``` 
please docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
9cdae062a0a6        nginx               "nginx -g 'daemon of…"   48 seconds ago      Up 46 seconds       0.0.0.0:80->80/tcp                  proxy
18fc8cb94328        httpd               "httpd-foreground"       2 minutes ago       Up 2 minutes        0.0.0.0:8080->80/tcp                webserver
cf91e90228f2        mysql               "docker-entrypoint.s…"   3 minutes ago       Up 3 minutes        0.0.0.0:3306->3306/tcp, 33060/tcp   db
```

# Test curl
```bash
e-dlx@edlx-virtual-machine:~/Desktop$ curl localhost 80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

```bash
e-dlx@edlx-virtual-machine:~/Desktop$ curl localhost:8080
<html><body><h1>It works!</h1></body></html>
```

# Stop Docker Container and Clean Docker Container
> docker container stop proxy webserver db
> 
> docker container rm 9cdae062a0a6 18fc8cb94328 cf91e90228f2

# Quiz
* Q: If you wanted to view running containers as well as containers that you have stopped and not removed, what command would you use?
    * docker container ls -a
    *  docker ps -a

* Q: What does the -d flag do in a docker run command?
    * It detaches the container to run in the background, and returns you to the shell prompt
    * it's the short version of --detach. By running in detached mode, we are able to have access to our command line when the container spins up and runs. Without it, we would have logs constantly fed onto the screen.

* Q: Would the following two commands create a port conflict error with each other?
    > docker container run -p 80:80 -d nginx
    > 
    > docker container run -p 8080:80 -d nginx

  * No, just because the containers are both listening on port 80 inside (the right number), there is no conflict because on the host they are published on 80, and 8080 separately (the left number).

* Q: I ran 'docker container run -p 80:80 nginx' and my command line is gone and everything looks frozen. Why?
  * Because you didn't specify the -d flag to detach it in the background, and there aren't any Nginx logs yet.
  * Everything is normal. You didn't specify the -d flag to detach it in the background, and there aren't any logs coming across the screen because you haven't connected to the published port yet, so Nginx has nothing to log about.


# What's Going On In Containers
* docker container top - process list in one container
* docker container inspect - details of one container config
* docker container stats - performance stats for all containers


> docker container run --detach --name nginx nginx
> 
> docker container run --detach --name mysql --env MYSQL_RANDOM_ROOT_PASSWORD=true mysql

# Docker Container Inspect
Show metadata about the container (startup config, volumn, networking, etc.)
> docker container inspect mysql
> 
> a

# Docker Container Stats
> docker container stats --help
> a
> docker container stats

# Getting a Shell Inside Containers
https://www.digitalocean.com/community/tutorials/package-management-basics-apt-yum-dnf-pkg

## docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]
* -i, --interactive                    Keep STDIN open even if not attached
* -t, --tty                            Allocate a pseudo-TTY

# start `new container` interactively
> docker container run -it 
>  
> docker container run -it --name proxy nginx bash  
> a



Its default CMD is `bash` so we don't have to specify it.
> docker container run -it --name ubuntu ubuntu

Continue using existing container
> docker container start -ai ubuntu


# run additional command in `Existing Container`
> docker container exec -it
> s
> docker container exec -it mysql bash


# Docker Pull
> docker pull <image name>
>
> docker pull alpine
> 
> docker image ls

# Run Docker Container from Image with shell
> docker container run -it alpine bash

```
Q: Why error "docker: Error response from daemon: OCI runtime create failed: container_linux.go:346: starting container process caused "exec: \"bash\": executable file not found in $PATH": unknown."
A: bash isn't in the container
```
> docker container run -it alpine sh

# Docker `Networks: Concepts`
* Review of docker container run -p
* For local dev/testing, networks usually "just work"
* Quick port check with docker container port <container>
* Learn concepts of Docker Networking
* Understand how network packets move around Docker

# Docker `Networks Defaults`
* Each container connected to a private virtual network "bridge"
* Each virtual network routes through NAT firewall on host IP
* All containers on a virtual network can talk to each other without -p
* Best practice is to create a new virtual network for each app:
  * network "my_web_app" for mysql and php/apache containers
  * network "my_api" for mongo and nodejs containers
* Batteries Included, But Removable"
  * Defaults work well in many cases, but easy to swap out parts to customize it
* Make new virtual networks
* Attach containers to more then one virtual network (or none)
* Skip virtual networks and use host IP (--net=host)
* Use different Docker network drivers to gain new abilities


#  Expose port (--publish, -p)
Remember pubishing  ports is always in `-p HOST:CONTAINER` format
> docker container run --publish 80:80 --name webhost --detach nginx
> 
> docker container port webhost

80/tcp -> 0.0.0.0:80

docker container inspect --format

# Format command and log output
## join
join concatenates a list of strings to create a single string. It puts a separator between each element in the list.

> docker inspect --format '{{join .Args " , "}}' container

## json🔗
json encodes an element as a json string.

> docker inspect --format '{{json .Mounts}}' container

## lower
lower transforms a string into its lowercase representation.

> docker inspect --format "{{lower .Name}}" container

## split
split slices a string into a list of strings separated by a separator.

> docker inspect --format '{{split .Image ":"}}'

## title
title capitalizes the first character of a string.

> docker inspect --format "{{title .Name}}" container

## upper
upper transforms a string into its uppercase representation.

> docker inspect --format "{{upper .Name}}" container

## println
println prints each value on a new line.

> docker inspect --format='{{range .NetworkSettings.Networks}}{{println .IPAddress}}{{end}}' container

## Hint
To find out what data can be printed, show all content as json:

> docker container ls --format='{{json .}}'

# Learn common container management commands


# Learn Docker networking basics


# Requirements: Have latest Docker installed from last Section

# Docker command line structure




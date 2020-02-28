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

# Format (--format)
A common options for formatting the output of commands using "Go templates"
> docker container inspect --format
> 
> docker container inspect --format "{{ .NetworkSettings.IPAddress }}" webhost

# FIXME: Change In Official Nginx Image Removes Ping
Hey just a quick note before doing the next few lectures. A recent June 2017 change in the official nginx image https://hub.docker.com/_/nginx (`nginx`  or `nginx:latest` ) removes ping, but I use it in the next few videos to test connectivity, so you might get an error about "ping not found". I'm working on updates to those videos but until I can get them processed and uploaded, just do this:

Anywhere I do a `docker container run <stuff> nginx` , where nginx  is the image you should use, replace that with nginx:alpine , which still has ping command in it.

# Docker Networks: CLI Management
* Show networks `docker network ls`
* Inspect a network `docker network inspect`
* Create a network `docker network create --driver`
* Attach a network to container `docker network connect`
* Detach a network from container `docker network disconnect`


> docker network ls
```
NETWORK ID          NAME                DRIVER              SCOPE
0df00346cfcd        bridge              bridge              local
c74a5a2b10bb        host                host                local
723d1c8aca70        none                null                local
```
> docker network inspect `bridge`

# (--network none)
Removes eth0 and only leaves you with localhost interface in container

# Create a network
> docker network create my_app_net
> 
> docker container run --detach --name new_nginx --network my_app_net nginx


# Plug and Unplug network
## plug interface to container
> docker network connect my_app_net webhost
> 
> docker container inspect webhost
```json
"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "0df00346cfcdda22aa5db3301dd3568c60ef84dad1eb392f1ac42efaa2f1ece8",
                    "EndpointID": "a2e338c9d6e847717de602deeae96151ef2076daede4da644d08cd5b0daf47b4",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:03",
                    "DriverOpts": null
                },
                "my_app_net": {
                    "IPAMConfig": {},
                    "Links": null,
                    "Aliases": [
                        "1952a567d687"
                    ],
                    "NetworkID": "d2d68c37e55d01128a895fd2c8e34ca1bcbe2c355473a0282aafadec068b7c0c",
                    "EndpointID": "3e4c27485b2db8ca170abb256effa49788797b49e7e48d6f6ec3355ee6e47bae",
                    "Gateway": "172.18.0.1",
                    "IPAddress": "172.18.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:12:00:03",
                    "DriverOpts": {}
                }
```

## Unplug interface to container
> docker network disconnect my_app_net webhost
> 
> docker container inspect webhost

# Docker Networks: Default Security
* Create your apps so frontend/backend sit on same Docker network
* Their inter-communication never leaves host
* All externally exposed ports closed by default
* You must manually expose via -p, which is better default security!
* This gets even better later with Swarm and Overlay networks

# Docker Networks: DNS
How container find each other 
* Understand how DNS is the key to easy inter-container comms
* See how it works by default with custom networks
* Learn how to use `--link` to enable DNS on default bridge network


# Docker `DNS`
Docker daemon has a built-in DNS server that containers use by default
> docker container run --detach --name my_nginx --network my_app_net nginx:alpine
> docker container run --detach --name my_nginx --network my_app_net nginx:alpine

## Ping docker
> docker container exec -it my_nginx ping new_nginx


# Docker Networks: DNS
* Containers shouldn't rely on IP's for inter-communication
* DNS for friendly names is built-in if you use custom networks
* You're using custom networks right?
* This gets way easier with Docker Compose in future Section

# Quiz
## Q1: Which command would we use to get an overview look of information about a specific container like Networks, IP address, mounts, and current status?
This shows you the container configuration. It can also be modified to only return a specific piece of data using the --format flag.
> docker container inspect

## Q2: In order to connect to a shell inside of a container, you should use docker ssh to get inside.
Correct! By using the 'docker exec -it <container> sh' (or bash) command on a container, we can connect to a shell from inside it.
> False

## Q3: If you wanted multiple containers to be able to communicate with each other on the same docker host, which network driver would you use?
Right, the default bridge network driver allow containers to communicate with each other when running on the same docker host.
> bridge

# Assignment Requirements: CLI App Testing
* Know how to use -it to get shell in container
* Understand basics of what a Linux distribution is like Ubuntu and CentOS
* Know how to run a container
* 


# Assignment: CLI App Testing
* Use different Linux distro containers to check curl cli tool version
* Use two different terminal windows to start bash in both centos:7 and ubuntu:14.04, using -it
* Learn the docker container run —rm option so you can save cleanup
* Ensure curl is installed and on latest version for that distro
* ubuntu: apt-get update && apt-get install curl
* centos: yum update curl
* Check curl --version

> docker container run --rm -it centos:7 bash //--rm Automatically remove the container when it exits
> yum update curl
> 
> docker container run --rm -it ubuntu:14.04 bash
> apt-get update && apt-get install curl -y

# Assignment Requirements: DNS RR Test
* Know how to use -it to get shell in container
* Understand basics of what a Linux distribution is like Ubuntu and CentOS
* Know how to run a container
* Understand basics of DNS records



# Assignment: DNS Round Robin Test
* Ever since Docker Engine 1.11, we can have multiple containers
on a created network respond to the same DNS address
* Create a new virtual network (default bridge driver)
* Create two containers from elasticsearch:2 image
* Research and use —network-alias search when creating them
to give them an additional DNS name to respond to
* Run alpine nslookup search with --net to see the two
containers list for the same DNS name
* Run centos curl -s search:9200 with --net multiple times
until you see both "name" fields show

> docker network create dude
> 
> docker container run --detach --net dude --net-alias search elasticsearch:2
> 
> docker container run --detach --net dude --net-alias search elasticsearch:2
> 
> docker container run --rm --net dude alpine nslookup
> 
> docker container run --rm --net dude centos curl -s search:9200


# Format command and log output
## join
join concatenates a list of strings to create a single string. It puts a separator between each element in the list.

> docker inspect --format '{{join .Args " , "}}' container

## json
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


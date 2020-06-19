#   CHAPTER 1.1 - Deploying Docker & Docker-Compose
>This chapter explains how to install ***Docker*** and ***Docker-Compose*** on your `Kali linux machine`

1.0 Installing Docker
====

|   |   |
|---|---|
| ![Screenshot Github](./assets/01-docker.png) | **Docker** container technology was launched in 2013 as an open source Docker Engine. It leveraged existing computing concepts around containers and specifically in the Linux world, primitives known as cgroups and namespaces. |
|   |   |

_"Containers are an abstraction at the app layer that packages code and dependencies together. Multiple containers can run on the same machine and share the OS kernel with other containers, each running as isolated processes in user space. Containers take up less space than VMs (container images are typically tens of MBs in size), can handle more applications and require fewer VMs and Operating systems."_

***Source: https://www.docker.com/products/docker-desktop*** 

Use your browser to connect to the ***[GUACAMOLE](https://guacamole.th.denarm.be/guacamole) server*** and select the __Linux SSH ELK__ connection login with your kali username and password ***(thadmin/PROVIDED_PASSWORD)***. First we're going to add the docker repository to our package repository on Kali.

![Screenshot Github](./assets/01-guacamole.jpg)
 
 ```code
apt-get update
apt-get install curl
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

```code
echo 'deb [arch=amd64] https://download.docker.com/linux/debian buster stable' | sudo tee /etc/apt/sources.list.d/docker.list
```

Next, let's install **docker community edition** and 

```code
 sudo apt-get update
 sudo apt-get install -y docker-ce
```

>A simpler way to do this is just running this script which will do all of the above =)

```code
curl -sSL https://get.docker.com/ | sh
```

Enable docker as a service so it starts up automatically with the operating system.

```code
 sudo systemctl status docker
 sudo systemctl enable docker
```

Finally we're going to allow docker to run wih the current non-root user (kali).

```code
sudo usermod -aG docker ${USER}
```





1.1 Installing Docker-Compose
====

|   |   |
|---|---|
| ![Screenshot Github](./assets/01-docker-compose2.png) | **Docker-compose** is a tool for defining and running multi-container Docker applications. |
|   |   |

_"We'll be using **docker-compose** files to deploy containers in most of our lab environment, so let's install docker-compose. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration."_

***source: https://github.com/docker/compose***

> **Don't install docker-compose with "apt-get install"**, this is an old version of docker-compose. Use the instructions below to install the latest version (we're hardcoding the version for our training purposes so things don't break.) 

The following command downloads docker compose version 1.25.0 to the /usr/local/bin directory - which is in the default PATH.

```yml
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

![Screenshot command](./assets/02-docker-compose-download.jpg)

Let's make this executable by adding the proper permissions to the file:

```yml
ls /usr/local/bin/docker-compose -lah
sudo chmod +x /usr/local/bin/docker-compose
docker-compose -version
```
![Screenshot command](./assets/01-docker-compose-chmod.jpg)

Docker Cheat Sheet
====
docker image ls -a  
docker system prune -a  
docker container logs "containername" --follow

BUILDING LOCAL IMAGES WITH DOCKER (`NOT LAB RELATED`)
====

 

``` 
git clone https://github.com/crimsoncore/docker_mdbook.git
cd docker_mdbook
docker build -t docker_mdbook:lean .
docker images
```

to change tags on an image:
```code
docker tag logstash_rest:8.0.0 logstash_rest:7.8.0
```

git clone your wiki and move to that folder:

```
docker run -it --name mdbook_lean -v $pwd:/opt/wiki -p3000:3000 docker_mdbook:lean
```

If you want to push the image to your docker hub:

```
docker login
docker push crimsoncorelabs/mdbook
```
CHAPTER 1.3: Installing MDBOOK and Wiki Instructions
====

>This chapter explains how to install the Threat Hunting WIKI and Labs guide

On your kali linux machine open a terminal. Use SSH and login with your kali username and password (***kali/kali***). First we're going to clone the MarkDown files from the wiki to our local machine> Then we'll clone the ThreatHunt github repository that contains all the labs and docker containers.

***Source: https://github.com/rust-lang/mdBook***

```code
 ssh kali@yourkalimachine
``` 

 ```code
 cd /opt
 sudo git clone https://github.com/crimsoncore/wiki.git
 sudo git clone https://gitlab.com/Schoonaert/threathunt.git
```
You should see the following structure for the MarkDown files:

![Screenshot command](./1.3/assets/01-gitclonewiki.jpg)

> Install **mdbook** (if you want to run this locally on your server) - in the Lab environment you can simple connect to the ELK server (10.0.0.5) on port 3000.

![Screenshot command](./1.3/assets/02-mdbook.jpg)

```code
cd /opt
sudo docker run -it --name mdbook -v /opt/wiki:/opt/wiki -p 3000:3000 -p 3001:3001 lschoonaert/mdbook
```

MDbook will automatically monitor for new updates, so you can pull down new versions of the documentation whenever available:

```code
cd /opt/wiki
sudo git pull
```

> We also provided docker compose files, which you can find in the /opt/threathunt/docker-compose directory. You can install docker compose files like this :

```code
cd /opt/threathunt/docker-compose
sudo docker-compose -f dc.mdbook.yml up -d
```
- -f : the name of the docker-compose name
- -d : run in daemon mode

**Source: /opt/threathunt/docker-compose/dc.mdbook.yml**
```yml
version: '2'
services:
  mdbook:
    image: lschoonaert/mdbook
    container_name: mdbook
    restart: unless-stopped
    volumes:
    - /opt/wiki:/opt/wiki
    ports:
      - 3000:3000
    networks:
      - elastic
networks:
  elastic:
    driver: bridge
```
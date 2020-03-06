CHAPTER 1: Threat Hunting Lab - MDBOOK and Wiki Instructions
====

This chapter explains how to install the Threat Hunting WIKI and Labs guide

> On your kali linux machine open a terminal. Use SSH and login with your kali and password (kali/kali). First we're going to add the docker repository to our package repository on Kali.

```yml
 ssh kali@yourkalimachine
 cd /opt
 git clone https://github.com/crimsoncore/wiki.git
```
You should see the following structure:

![Screenshot command](./0/assets/01-gitclonewiki.jpg)

> Install **mdbook** (if you want to run this locally on your server) - in the Lab environment you can simple connect to the ELK server (10.0.0.5) on port 3000.
> 
```yml
cd /opt
sudo docker run -it --name mdbook -v /opt/wiki:/opt/wiki -p 3000:3000 -p 3001:3001 lschoonaert/mdbook
```

MDbook will automatically monitor for new updates, so you can pull down new versions of the documentation whenever available:

```yml
cd /opt/wiki
sudo git pull
```

> We also provided docker compose files, which you can find in the /opt/threathunt/docker-compose directory. You can install docker compose files like this :

```yml
cd /opt/threathunt/docker-compose
sudo docker-compose -f dc.mdbook.yml up -d
```
- -f : the name of the docker-compose name
- -d : run in daemon mode


Source: /opt/threathunt/docker-compose/dc.mdbook.yml
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
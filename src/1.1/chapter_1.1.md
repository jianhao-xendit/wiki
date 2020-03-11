#   CHAPTER 1.1: Deploying Docker & Docker-Compose
>This chapter explains how to install the Docker and Docker-Compose on your Kali linux machine

![Screenshot command](./assets/01-docker.png)

***Source: https://www.docker.com/products/docker-desktop*** 

On your kali linux machine open a terminal. Use SSH and login with your kali and password (kali/kali). First we're going to add the docker repository to our package repository on Kali.

```yml
 ssh kali@yourkalimachine
 ```
 
 ```yml
 curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

```yml
 echo 'deb [arch=amd64] https://download.docker.com/linux/debian buster stable' | sudo tee /etc/apt/sources.list.d/docker.list
 ```

Next, let's install **docker community edition** and enable it as a service so it starts up automatically with the operating system.

```yml
 sudo apt-get update
 sudo apt-get install -y docker-ce
 sudo systemctl status docker
 sudo systemctl enable docker
```

Finally we're going to allow docker to run wih the current non-root user (kali).

```yml
sudo usermod -aG docker ${USER}
```

We'll be using **docker-compose** files to deploy containers in most of our lab environment, so let's install docker-compose. 

> **Don't install docker-compose with "apt-get install"**, this is an old version of docker-compose. Use the instructions below to install the latest version (we're hardcoding the version for our training purposes so things don't break.) 

>*source: https://github.com/docker/compose*

![Screenshot command](./assets/01-docker-compose.png)

The following command downloads docker compose version 1.25.0 to the /usr/local/bin directory - which is in the default PATH.

```yml
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

![Screenshot command](./assets/01-docker-compose-download.jpg)

Let's make this executable by adding the proper permissions to the file:

```yml
ls /usr/local/bin/docker-compose -lah
sudo chmod +x /usr/local/bin/docker-compose
docker-compose -version
```
![Screenshot command](./assets/01-docker-compose-chmod.jpg)

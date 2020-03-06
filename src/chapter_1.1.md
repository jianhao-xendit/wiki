#   CHAPTER 1.1 Deploying Docker
>This chapter explains how to install the Docker and Portaineron your Kali linux machine

On your kali linux machine open a terminal. Use SSH and login with your kali and password (kali/kali). First we're going to add the docker repository to our package repository on Kali.

```yml
 ssh kali@yourkalimachine
 cd /opt
 sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

 ```

Next, let's install docker community edition and enable it as a service so it starts up automatically with the operating system.

```yml
 sudo apt-get update
 sudo apt-get install -y docker-ce
 sudo systemctl status docker
 sudo systemctl enable docker
```

Finally we're going to allow docker to run wih the current non-root user (kali).

```yml
sudo usermod -aG docker ${USER}
su - ${USER}
id -nG
```


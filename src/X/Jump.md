# Chapter X.3 - Setting up the initial Jump Host
>This chapter explains how to install your initial deployment server (`Jump`) that is used to deploy the ***terraform*** to deploy infrastructure, and then deploy packages and configurations with ***ansible***

docker run -it -p 127.0.0.1:8080:8080 -v "/opt:/home/coder/project" crimsoncorelabs/coder:latest

git clone git@github.com:crimsoncore/ansible_jump.git

from your PC:
ssh-copy-id -i ~/.ssh/id_rsa thadmin@az-jump-lsazure.westeurope.cloudapp.azure.com

In VSC coder web gui
git config --global user.name "luks_coder"
git config --global user.email "luks@crimsoncore.be"
git config --global --list
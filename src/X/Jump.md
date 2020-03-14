# Chapter X.3 - Setting up the initial Jump Host
>This chapter explains how to install your initial deployment server (`Jump`) that is used to deploy the ***terraform*** to deploy infrastructure, and then deploy packages and configurations with ***ansible***

docker run -it --name coder -p 127.0.0.1:8080:8080 -v "/opt:/home/coder/project" crimsoncorelabs/coder:latest

git clone git@github.com:crimsoncore/ansible_jump.git

from your PC:
ssh-copy-id -i ~/.ssh/id_rsa thadmin@az-jump-lsazure.westeurope.cloudapp.azure.com

In VSC coder web gui
git config --global user.name "luks_coder"
git config --global user.email "luks@crimsoncore.be"
git config --global --list

> when you use git (cline, pull, push) do ***not*** use sudo, this will fail since the public SSH cert is copied under the current user's .ssh folder. Running with sudo will use ROOT privileges and no cert is associated with this account.

Always clone with SSH, if you cloned with HTTPS by accident use this :

git remote set-url origin git@github.com:crimsoncore/`reponame`.git

in your VSC coder browser GUI
cd projects
then git clone your code there

ANSIBLE jump
ansible-playbook -i pj-production -l jump site.yml --tags jump
ansible-playbook -i hosts site.yml --list-tasks

Visual Studio Code CODE-SERVER shortcuts
cmd + ` = open terminal

BYOBU

MAC VISUAL STUDIO CODE
    add extension -> Remote SSH
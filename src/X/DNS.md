# Chapter X.5 - DNS

>This chapter explains how to set up your DNS Records to work with the lab environment,

1.1 CREATE DNS RECORDS (EURODNS) for `RABBITMQ`
====

rabbitmq.`th.crimsoncorelabs.be` -> az-mq-lsazure.westeurope.cloudapp.azure.com.  

portainer-mq.`th.crimsoncorelabs.be` -> az-mq-lsazure.westeurope.cloudapp.azure.com.  
guacamole.`th.crimsoncorelabs.be` -> az-mq-lsazure.westeurope.cloudapp.azure.com.   
traefik-mq.`th.crimsoncorelabs.be` -> az-mq-lsazure.westeurope.cloudapp.azure.com.

1.1 CREATE DNS RECORDS (EURODNS) for `ELK`
====  
elk.`th.crimsoncorelabs.be` -> az-elk-lsazure.westeurope.cloudapp.azure.com.  

portainer-elk.`th.crimsoncorelabs.be` -> az-elk-lsazure.westeurope.cloudapp.azure.com.  
kibana.`th.crimsoncorelabs.be` -> az-elk-lsazure.westeurope.cloudapp.azure.com.  
cerebro.`th.crimsoncorelabs.be` -> az-elk-lsazure.westeurope.cloudapp.azure.com.
wiki.`th.crimsoncorelabs.be` -> az-elk-lsazure.westeurope.cloudapp.azure.com.  
traefik-elk.`th.crimsoncorelabs.be` -> az-elk-lsazure.westeurope.cloudapp.azure.com.

1.3 CREATE DNS RECORDS (EURODNS) for `CODER`
====
coder.crimsoncorelabs.be -> az-jump-lsazure.westeurope.cloudapp.azure.com.  
traefik.crimsoncorelabs.be -> az-jump-lsazure.westeurope.cloudapp.azure.com.  
portainer.crimsoncorelabs.be -> az-jump-lsazure.westeurope.cloudapp.azure.com.  
jupyter.crimsoncorelabs.be -> az-jump-lsazure.westeurope.cloudapp.azure.com.  

Additional changes to your `JUMP (CODER) host`
====

remove az-jump-lsazure.westeurope.cloudapp.azure.com from ./ssh/known_hosts

cd project_jump  
git clone git@github.com:crimsoncore/terraform_jump.git  
git clone git@github.com:crimsoncore/ansible_jump.git  

```code
docker run -it -p 127.0.0.1:8080:8080 -v "$PWD:/home/coder/project" -v "$HOME/.ssh:/home/coder/.ssh" -e PASSWORD=Password1234! crimsoncorelabs/coder:latest
```

open browser localhost:8080

project -> terraform_jump
right click -> open in terminal

az login

nano terraform.tfvars


dns_prefix = “lsazure"

cd project -> ansible_jump  
mkdir group_vars  
cd group_vars  

nano all.yml

```yml
# Traefik container general variables
traefik:
name: traefik
image: traefik:v2.1
domain: crimsoncorelabs.be
subdomain: traefik
file_location: /opt/containers/traefik/
acme_email: luks@crimsoncore.be
acme_certresolver: "letsencrypt"
basic_auth: "thadmin:$apr1$2RnW3J50$bjzbmnFizHfUNlegGHFAN0"

# Portainer variables
portainer:
name: portainer
image: portainer/portainer
subdomain: portainer
volume: portainer
# Only useful if you would like to set the admin password at bootup, check portainer documentation
adminpw_command: --admin-password $2y$05$1Kj0QrYF1QVGRf2MSdhjYOvhaWqoxN17YvULcNqrECMgiUOe5IwyC
acme_certresolver: "letsencrypt"

# oauth configuration skip if not neccesary -> adjust the authentication in code-server and traefik role to basic-auth from forward-auth
oauth:
name: oauth
image: thomseddon/traefik-forward-auth
subdomain: oauth
acme_certresolver: letsencrypt-staging
PROVIDERS_GOOGLE_CLIENT_ID: "CLIENT_ID"
PROVIDERS_GOOGLE_CLIENT_SECRET: "CLIENT_SECRET"
SECRET: "Random secret"
COOKIE_DOMAIN: "sub.example.com"
INSECURE_COOKIE: "false"
DOMAIN: "sub.example.com"
AUTH_HOST: "oauth.sub.example.com"
URL_PATH: "/_oauth"
WHITELIST: "user@gmail.com" # If you have a gmail account enter your account, else all gmail users have access
LOG_LEVEL: "info"
LIFETIME: "36000"

# Jupyter configuration
jupyter:
subdomain: jupyter

# Code server configuration
codeserver:
name: code-server
image: crimsoncorelabs/coder:latest
subdomain: coder
volume: code-server
acme_certresolver: "letsencrypt"
file_location: /opt/containers/code-server/
rsa_private_key: |
-----BEGIN RSA PRIVATE KEY-----
_YOUR PRIVATE SSH KEY_
-----END RSA PRIVATE KEY-----  
rsa_public_key: "ssh-rsa corresponding public key add to github/gitlab authorized keys"
# Add every extension on a different line, this script will run once the container is started
runsh_extensions: |
code-server --install-extension mauve.terraform
# Adds the correct righs to the .ssh folder | Copy the ssh-keyscan line for every git provider you use (eliminates prompting)
runsh_prepssh: |
sudo chown coder:coder ~/.ssh
ssh-keyscan -H gitlab.com >> ~/.ssh/known_hosts
# Configure git global settings and clone the repositories you want.
runsh_git: |
git config --global user.name "luks"
git config --global user.email "luks@crimsoncore.be"
# git clone -q git@gitlab.com:example/example.git
```

hosts_generic -> copy to new hosts

```yml
[jump]
az-jump-lsazure.westeurope.cloudapp.azure.com

[jump:vars]
ansible_user=thadmin
ansible_ssh_pass=Password1234!
ansible_ssh_extra_args='-o StrictHostKeyChecking=no’
```

cd terraform  
terraform init  
terraform plan  
terraform apply  
yes

open in browser - coder.crimsoncorelabs.be:8080
https://portainer.crimsoncorelabs.be/#/containers

sudo chown -R coder:coder project  
mkdir project_threathunt

git clone git@github.com:crimsoncore/threathunt.git  
git clone git@github.com:crimsoncore/wiki.git  
git clone git@github.com:crimsoncore/terraform_threathunt.git  
git clone git@github.com:crimsoncore/ansible_threathunt.git  
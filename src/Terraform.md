# Chapter X.1 - Terraform

On your windows/mac machine install

  - `Visual Studio Code (VSC)`
  - https://code.visualstudio.com/#alt-downloads
  

![Screenshot command](./X/assets/02-VSC.png)

```code
cd /opt
git clone https://gitlab.com/Schoonaert/threathuntterraform.git
```

Open this folder in `VSC`, it will prompt you to open it again but as a container. Then in the terminal at the bottom login to you Azure instance with your credentials:

```code
az login
```

```code
sudo terraform init
sudo terraform plan
```
>use a unique prefix "lsazure", this will be used to assign FQDN names for the virtual machines public IP addresses

```code
sudo terraform apply
```

Since we are deploying some images from the azure market place, we need to accept the EULA before kicking off the scripts. In your DEV container in Visual Studio Code, type the following in the terminal at the bottom:

```code
az vm image terms accept --urn kali-linux:kali-linux:kali:2019.2.0
```
![Screenshot command](./X/assets/01-EULA.jpg)
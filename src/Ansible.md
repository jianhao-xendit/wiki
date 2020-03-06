# Chapter X.2 - Ansible

On your windows/mac machine install

- Visual Studio Code
  - https://code.visualstudio.com/#alt-downloads
  


```code
cd /opt
git clone https://gitlab.com/pblaton/threathuntansible.git
```

Open this folder in VSC, it will prompt you to open it again but as a container.

**"domain"** will make the Windows 10 Workstations join the acme.local domain.

```code
ansible-playbook -i ls-production -l ws site.yml --tags domain
```

**"init"** will create student accounts on the DC, amd install all the software packages (git, winlogbeat, processhacker, sysmon)

```code
ansible-playbook -i ls-production -l dc site.yml --tags init
```
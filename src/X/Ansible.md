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

**"init"** on DC will create student accounts on the DC, and install all the software packages (git, winlogbeat, processhacker, sysmon). This will also configure winlogbeat on the DC to start forwarding messages to the Logstash service (listening on PORT 5044) on the RabbitMQ server (IP: 10.0.0.6 )..

```code
ansible-playbook -i ls-production -l dc site.yml --tags init
```

**"init"** on WS will install all the software packages (git, winlogbeat, processhacker, sysmon) on the student workstations.

```code
ansible-playbook -i ls-production -l ws site.yml --tags init
```
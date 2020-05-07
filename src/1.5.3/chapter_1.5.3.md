#   Chapter 1.5.3 - Auditd

Install
====

```code
apt-get update  
sudo apt-get install auditd audispd-plugins  
service auditd start  
service auditd status  
```

/var/log/audit/audit.log  

tail -f audit.log

Configure
====


-w: This stands for where, and it points to the object that we want to monitor. In this case, it's /etc/passwd.  

-p: This indicates the object's permissions that we want to monitor. In this case, we're monitoring to see when anyone either tries to (w)rite to the file or tries to make (a)ttribute changes. (The other two permissions that we can audit are (r)ead and e(x)ecute.)  

-k: The k stands for key, which is just auditd's way of assigning a name to a rule. So, passwd_changes is the key, or the name, of the rule that we're creating.  

to monitor access to specific files  

```code
sudo auditctl -w /opt/cobaltstrike/startcobalt.sh
sudo auditctl -w /etc/passwd -p wa -k passwd_changes
sudo auditctl -w /etc/ssh/sshd_config -p rwxa -k sshconfigchange
sudo auditctl -w /opt/ -k opt_dir_monitor

service auditd reload
tail -f audit.log 
```

-k = filter key on an audit rule

sudo ausearch -i -k passwd_changes
sudo ausearch -i -k passwd_changes | grep test
sudo ausearch -i -k passwd_changes | grep SYSCALL
sudo ausearch -i -k opt_dir_monitor | grep proctitle

if you restart the auditd service - these rules will be deleted.  

nano /etc/audit/rules.d/audit.rules


to log usernames instead of uid (user id's)

nano /etc/audit/auditd.conf
change log_format = RAW to log_format = ENRICHED

List the active rules:  

```code
sudo auditctl -l
```

AUTH Log
====

/var/log/auth.log



***Resources:***  
https://www.digitalocean.com/community/tutorials/how-to-use-the-linux-auditing-system-on-centos-7
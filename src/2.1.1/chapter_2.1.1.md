# Chapter 2.1.1 - BloodHound 

>This chapter explains how to install Bloodhound on your `Kali linux machine`


_"BloodHound uses graph theory to reveal the hidden and often unintended relationships within an Active Directory environment. Attackers can use BloodHound to easily identify highly complex attack paths that would otherwise be impossible to quickly identify. Defenders can use BloodHound to identify and eliminate those same attack paths. Both blue and red teams can use BloodHound to easily gain a deeper understanding of privilege relationships in an Active Directory environment."_

![Screenshot Github](./assets/01-bloodhound.png)

Installing Bloodhound
===

- SOURCE : ***[https://github.com/BloodHoundAD/BloodHound](https://github.com/BloodHoundAD/BloodHound)*** 

On your kali linux machine

```code
sudo apt-get update
sudp apt-get install bloodhound
```

then configure Neo4j:

```code
sudo neo4j console
```

Navigate to http://localhost:7474/ to set up a DB user account by changing default passwords from neo4j:neo4j to something else - we will need those credentials when launching BloodHound itself.

SOURCE : https://ired.team/offensive-security-experiments/active-directory-kerberos-abuse/abusing-active-directory-with-bloodhound-on-kali-linux

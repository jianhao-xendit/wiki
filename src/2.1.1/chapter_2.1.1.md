# Chapter 2.1.1 - BloodHound 

Installing Bloodhound
===
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

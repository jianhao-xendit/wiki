# Chapter 2.3 Credential Access
----
Mitre: T1003 
--
***source: https://attack.mitre.org/techniques/T1003/***

>Credential dumping is the process of obtaining account login and password information, normally in the form of a hash or a clear text password, from the operating system and software. Credentials can then be used to perform Lateral Movement and access restricted information.

- **lsass memory** :
- **SAM** Registry hive :
- **SECURITY** Registry hive
- **NTDS.dit** file: hashes of domain accounts, present on Domain Controllers.
- **SYSTEM** Registry hive : SysKey,to decrypt:
    -  SAM
    -  LSASecrets
    -  Cachedcredentials
    -  NTDS.dit
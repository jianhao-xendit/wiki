# Chapter 2.4 - Kerberoasting 
----
Mitre: T1208
--
***source: https://attack.mitre.org/techniques/T1208/***

>Kerberoasting attacks allow any valid domain account to request a Kerberos service ticket for any service and then use the ticket for offline password cracking attempts. 

_"Each domain user can request a TGS from a domain controller for any service that has a registered SPN. When the TGS is created, the domain controller does not check whether the requesting user is authorized to access the respective resource. Verifying credentials is left up to the service set up to handle this task in the Kerberos implementation in Windows. A hacker can use this ticket offline to figure out the password for the service account because the ticket has been encrypted with the NT hash of the service account."_

> Host-based accounts are of no use in Kerberoasting attacks, because a computer account in Active Directory has a randomly generated 128-character long password which is changed every 30 days.

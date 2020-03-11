# Chapter 2.4.2 - Building the detection 
----
> Mitre: T1208 - Kerberoast

> **LOG SOURCES**:
> - EID 4679 - Kerberos Service Request
> - Log : windows security eventlog
> - Collected on:  Domain Controller
> - Reference : https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4769
> 
> 

Sigma Rule:
```yaml
title: KERBEROAST Activity
status: production
description: 'Detects the attack technique KERBEROAST which is used to move laterally inside the network'
references:
    - https://www.trustedsec.com/2018/05/art_of_kerberoast/
    - https://attack.mitre.org/techniques/T1208/
author: TrustedSec (method) / Luk Schoonaert (rule)
tags:
    - attack.credential access
    - attack.t1208
logsource:
    product: windows
    service: security
    definition: 
detection:
    selection:
        - EventID: 4769
          Status: "0x0"
          TicketEncryptionType: "0x17"
          TicketOptions: "*08100*"
    filter:
        ServiceName : ”krbtgt”
        ServiceName : "*$"
        ServiceName : "*$@*"
    condition: selection and not filter
falsepositives:
    - Administrator activity
    - Penetration tests
level: medium
```
Sigma rule generation:

```code
sigmac -I -t es-qs -c /opt/sigma/tools/config/winlogbeat.yml /opt/threathunt/sigma_rules/win_crimsoncore_kerberoast.yml 
```

Kibana Query

```code
(event_id : 4769 and event_data.Status:"0x0" and event_data.TicketEncryptionType : "0x17" and not event_data.ServiceName :”krbtgt” and not event_data.ServiceName:*$ and not event_data.ServiceName:*$@* and event_data.TicketOptions :*08100*)
```
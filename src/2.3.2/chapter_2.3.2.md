# Chapter 2.3.2 - CREDENTIAL DUMPING - Building the detection 
----
> Mitre: [T1003](https://attack.mitre.org/techniques/T1003/) - Credential Dumping

![Screenshot T1003](./assets/01-T1003.jpeg)

1.0 DETECTING MIMIKATZ (LSASS INJECTION)
----
> **LOG SOURCES**:
> - EID 10 (SYSMON) - Process Access

```yaml
title: Mimikatz Detection LSASS Access
status: experimental
description: Detects process access to LSASS which is typical for Mimikatz (0x1000 PROCESS_QUERY_ LIMITED_INFORMATION, 0x0400 PROCESS_QUERY_ INFORMATION "only $
    versions", 0x0010 PROCESS_VM_READ)
references:
    - https://cyberwardog.blogspot.com/2017/03/chronicles-of-threat-hunter-hunting-for_22.html
tags:
    - attack.t1003
    - attack.s0002
    - attack.credential_access
logsource:
    product: windows
    service: sysmon
detection:
    selection:
        EventID: 10
        TargetImage: 'C:\windows\system32\lsass.exe'
        GrantedAccess:
            - '0x1410'
            - '0x1010'
    condition: selection
falsepositives:
    - unknown
level: high
```

Sigma rule generation:

```code
sigmac -I -t es-qs -c /opt/sigma/tools/config/winlogbeat-modules-enabled.yml /opt/threathunt/sigma_rules/win_crimsoncore_lsass_dump.yml
```

Kibana Query

```code
(winlog.channel:"Microsoft-Windows-Sysmon/Operational" AND winlog.event_id:"10" AND winlog.event_data.TargetImage: "C:\Windows\system32\lsass.exe" AND winlog.event_data.GrantedAccess:("0x1410" OR "0x1010"))
```

2.0 DETECTING LSASS Process Dumping
----
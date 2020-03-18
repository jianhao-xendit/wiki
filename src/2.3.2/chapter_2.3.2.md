# Chapter 2.3.2 - CREDENTIAL DUMPING - Building the detection 
----
> Mitre: [T1003](https://attack.mitre.org/techniques/T1003/) - Credential Dumping

![Screenshot T1003](./assets/01-T1003.jpeg)



> **LOG SOURCES**:
> - EID 10 (SYSMON) - Process Access

```yaml


```


Sigma rule generation:

```code
sigmac -I -t es-qs -c /opt/sigma/tools/config/winlogbeat.yml /opt/threathunt/sigma_rules/win_crimsoncore_lsass_dump.yml 
```

Kibana Query

```code
(winlog.channel:"Microsoft\-Windows\-Sysmon\/Operational" AND winlog.event_id:"10" AND winlog.event_data.TargetImage:"C\:\\windows\\system32\\lsass.exe" AND winlog.event_data.GrantedAccess:"0x1fffff" AND winlog.event_data.CallTrace.keyword:(*dbghelp.dll* OR *dbgcore.dll*))
```

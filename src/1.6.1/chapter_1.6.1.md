#   Chapter 1.6.1 - ELK

Index aliases
----

PUT /winlogbeat-*/_alias/all_logs
PUT /logstash-*/_alias/all_logs
GET /_cat/aliases

Winlogbeat templates:
----

.\winlogbeat.exe export template --es.version 7.6.2 | Out-File -Encoding UTF8 winlogbeat.template.json
Invoke-RestMethod -Method Put -ContentType "application/json" -InFile winlogbeat.template.json -Uri http://localhost:9200/_template/winlogbeat-7.6.2

https://www.elastic.co/guide/en/beats/winlogbeat/current/winlogbeat-template.html
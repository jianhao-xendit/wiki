#   Chapter 1.6.1 - ELK


![Screenshot command](./assets/01-ELK-Stack.png)

- SOURCE : ***[https://www.elastic.co/elasticsearch/](https://www.elastic.co/elasticsearch/)*** 

>Virtual memory - Elasticsearch uses a mmapfs directory by default to store its indices. The default operating system limits on mmap counts is likely to be too low, which may result in out of memory exceptions.

On your `Kali linux machine` do the following

```code
sudo nano /etc/sysctl.conf
vm.max_map_count=262144
Reboot
```




Index aliases
----

```code
PUT /winlogbeat-*/_alias/all_logs  
PUT /logstash-*/_alias/all_logs  
GET /_cat/aliases  
```

```yml
POST /_aliases?pretty
{
    "actions" : [
        { "remove" : { "index" : "winlogbeat-*", "alias" : "alias1" } }
    ]
}
```

Winlogbeat templates:
----

.\winlogbeat.exe export template --es.version 7.6.2 | Out-File -Encoding UTF8 winlogbeat.template.json  
Invoke-RestMethod -Method Put -ContentType "application/json" -InFile winlogbeat.template.json -Uri http://localhost:9200/_template/winlogbeat-7.6.2

https://www.elastic.co/guide/en/beats/winlogbeat/current/winlogbeat-template.html


----

curl -XDELETE localhost:9200/logstash-alsid*
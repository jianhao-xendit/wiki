#   Chapter 1.9 - Alerting

Manual Install of elastalert on `Kali Linux Machine`
====

>we'll be skipping these steps, and instead install ElastALert as a docker container on your Kali Linux machine, where also you ELK stack is running in docker.

apt install python3-pip  
python3 -m pip install pip --upgrade && python3 -m pip install wheel  
sudo -H pip3 install --ignore-installed PyYAML  
python3 -m pip install elastalert==0.2.1  

mkdir /opt/elastalert  
mkdir /opt/elastalert/rules  
mkdir /opt/elastalert/log  
mkdir /opt/elastalert/config    
nano /opt/elastalert/config/config.yaml    

```yml
rules_folder: rules
run_every:
    seconds: 30
buffer_time:
    seconds: 45
es_host: es01
es_port: 9200
alert_time_limit:
    days: 1
writeback_index: elastalert_status
alert_text: "Index: {0} \nEvent_Timestamp: {1} \nBeat_Name: {2} \nUser_Name: {3} \nHost_Name: {4} \nLog_Name: {5} \nOriginal_Message: \n\n{6}"
alert_text_type: alert_text_only
alert_text_args: ["_index","@timestamp","beat.name","user_name","host_name","log_name","z_original_message"]
```

```python 
python3 -m elastalert.create_index --index elastalert_status --config /opt/elastalert/config.yaml
```

Make a sigma elastalert rule:

cd /opt
git clone https://github.com/crimsoncore/sigma.git

edit the sigma mapping:  
nano /opt/sigma/tools/config/winlogbeat.yml

add :

OriginalFileName: winlog.event_data.OriginalFileName

```code 
sigmac -Okeyword_blacklist=* -t es-qs -c /opt/sigma/tools/config/winlogbeat.yml /opt/sigma/rules/netview2.yml
```

result query:  
>(winlog.channel:"Microsoft\-Windows\-Sysmon\/Operational" AND winlog.event_id:"1" AND winlog.event_data.OriginalFileName:("net1.exe"))

CREATE ELASTALERT rule

```code
sigmac --target elastalert --config /opt/sigma/tools/config/winlogbeat.yml --output /opt/elastalert/rules/netview.yaml /opt/sigma/rules/netview2.yaml
```

_don't forget to change the index from winlogbeat-* to logstash, or change this in your MAPPING file (/opt/sigma/tools/config/winlogbeat.yml)_

```code
python3 -m elastalert.elastalert --config /opt/elastalert/config.yaml --rule /opt/elastalert/rules/netview.yaml --verbose --start $(date +"%Y-%m-%d")
```

go to your kibana and create elast-alert indexes

Elast-Alert docs
https://elastalert.readthedocs.io/en/latest/index.html

```yml
#alert:
#- debug
description: Detects recon using net.exe commands
filter:
- query:
    query_string:
      query: (winlog.channel:"Microsoft\-Windows\-Sysmon\/Operational" AND winlog.event_id:"1"
        AND winlog.event_data.OriginalFileName:("net1.exe"))
index: logstash-*
name: Net-commands_0
priority: 2
realert:
  minutes: 0
type: any

alert_text: "{2} Endpoint: {0} CommandLine: {1} Link: {3}"
alert_text_type: alert_text_only
alert_text_args:
- host.name
- winlog.event_data.CommandLine
- "@timestamp"
- kibana_link

#<a href='{3}'>Kibana link</a>

alert:
- "slack"
slack:
slack_webhook_url: "https://hooks.slack.com/services/pasteyourhookhere"
```

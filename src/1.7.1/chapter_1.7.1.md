#   Chapter 1.7 - Winlogbeat
>This chapter explains how to install the Winlogbeat on your `Windows 10 Machine`, and ship logs to the RabbitMQ server.

***RabbitMQ*** is a message queue that is used in this class to collect all the students winlogbeat logs, through a logstash instance.

The pipeline looks like this:

![Screenshot command](./assets/01-studentpipe.jpg)

From your Windows 10 Machine's Winlogbeat Agent -> Logstash (on RabbitMQ server) -> RabbitMQ (on RabbitMQ server). Later on we will configure the student ELK servers to pull the logs from their queue.

- logstash listens on **TCP 5044**
- RabbitMQ listens on **TCP 5672**

open powershell and run as ___administrator___:

```code
mkdir c:\temp
(New-Object System.Net.WebClient).DownloadFile("https://artifacts.elastic.co/downloads/beats/winlogbeat/winlogbeat-7.6.2-windows-x86_64.zip", "C:\temp\winlogbeat-7.6.2-windows-x86_64.zip")
Expand-Archive C:\temp\winlogbeat-7.6.2-windows-x86_64.zip -DestinationPath "C:\program files\"
cd "C:\program files\winlogbeat-7.6.2-windows-x86_64"
dir
```
now install winlogbeat, use powershell copy the 

```code
cp C:\threathunt\labsetup\winlogbeat.yml 'C:\Program Files\winlogbeat-7.6.2-windows-x86_64\'
.\install-service-winlogbeat.ps1
start-service winlogbeat
get-service winlogbeat
```

Now let's make sure the config file is correct, edit the ***winlogbeat.yml*** and check that the Logstash output is uncommented and pointing to the RabbitMQ server's (10.0.0.6) Logstash instance:

![Screenshot command](./assets/01-winlogbeat_mq.jpg)  

> You can check the logs in the hidden folder C:\ProgramData\winlogbeat\logs 

![Screenshot command](./assets/hidden.jpg)  

or test the config 

```code
.\winlogbeat.exe test output -e -d "*"
```

or 

```code
.\winlogbeat.exe test config -c .\winlogbeat.yml -e
```

Connect with `GUACAMOLE SSH` to your Kali Linux, select the right student number that was assigned to you in the beginning of the class:

> **NOTE**: The username and password for the Guacamole server are ***"thadmin" / "PROVIDED_PASSWORD"***. For the RDP/SSH connection your username __and__ password are studentxx. So if you are ***"student04"***, both your username and password for the windows machine will be ***"student04"***.

**Guacamole Username: thadmin**  
**Guacamole Password : PROVIDED_PASSWORD**

![Screenshot command](./assets/02-guacamole.jpg)

Let's start by opening powershell open your Windows 10 machine by clicking on the windows logo in the bottom left corner, and just start typing "power":

![Screenshot command](./assets/04-powershell.jpg)

Open your web browser (preferably chrome) and surf to http://kibana-az-elk-`lsazure`.westeurope.cloudapp.azure.com/

You will be prompted to authenticate:

**Username:**
```code
test
```
**Password:**
```code
test
```
In Kibana:

1. Click on ***"Management"***, the little cogs in the left bottom corner,
2. then click on ***"Index Patterns"***,
3. and finally click on ***"Create index pattern"***

![Screenshot command](./assets/03-kibanaindex.jpg)

1. Enter the index name "logstash-windows*"
2. Click "Next Step"

![Screenshot command](./assets/03-kibanadefine.jpg)

1. Select the "time filter field" and select "@timestamp"
2. Click on "Create index pattern"

![Screenshot command](./assets/03-kibanatime.jpg)

You have now created your Elastic index, and you will see a screen similar to this:

![Screenshot command](./assets/03-kibanacreated.jpg)

TROUBLESHOOTING WINLOGBEAT CONFIG FILES
====
.\winlogbeat.exe test output -e -d "*"  

maybe needed for Elastic SIEM:  

```code
Invoke-RestMethod -Method Put -ContentType "application/json" -InFile winlogbeat.template.json -Uri http://10.0.0.6:9200/_template/logstash-winlogbeat
```

Load Elasticsearch templates   
----
If Winlogbeat has a direction connection and is using Elasticearch as the output, it will automatically load the template. However, if you are using Logstash as the output, you need to manually load the Elasticsearch template. See example command to load the Elasticsearch template manually below;

```code
.\winlogbeat.exe setup --index-management -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["192.168.43.104:9200"]'
```

Setup Kibana Dashboards
----

To load Winlogbeat default visualization dashboards, you need to have created the index pattern. Hence, navigate to Kibana and create the Winlogbeat index pattern.  

Next, if you are using Elasticsearch as your output, you can load the dashboards by running the setup command or enabling dashboard loading in the winlogbeat.yml (setup.dashboards.enabled: true) configuration.

```code
cd C:\'Program Files'\Winlogbeat>
.\winlogbeat.exe setup --dashboards
```

Loading dashboards (Kibana must be running and reachable)  

OR

```code
#============================== Dashboards =====================================
# These settings control loading the sample dashboards to the Kibana index. Loading
# the dashboards is disabled by default and can be enabled either by setting the
# options here or by using the `setup` command.
#setup.dashboards.enabled: false
setup.dashboards.enabled: true
```
If you are using Logstash as the output, run the command below to load the dashboards. 

```code
cd C:\'Program Files'\Winlogbeat>  
.\winlogbeat.exe setup -e -E output.logstash.enabled=false -E output.elasticsearch.hosts=['10.0.0.5:9200']
```
***source: https://kifarunix.com/send-windows-logs-to-elastic-stack-using-winlogbeat-and-sysmon/***






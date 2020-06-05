#   Chapter 1.6.2 - Logstash

>This chapter explains how to install Logstash on your `Kali linux machine`


The Logstash Pipeline
=====

```code
cd /opt/threathunt/logstash
```

You will find 3 directories here  

![Screenshot command](./assets/01-logtstash_dirs.jpg)

1. config holds the logstash.yml file which for this lab we won't be using. You could for example configure ***[multiple pipelines](https://www.elastic.co/guide/en/logstash/current/multiple-pipelines.html)*** here.

2. logstash_enrich holds some input, filter and output scripts that we can use as examples - they are __not__ active in the configuration.  

3. the ***pipeline*** folder holds all the scripts for 
    - Logstash inputs for accepting logs, you can look at these as a listener on a specific TCP/UDP port.
    - Filters allow you to manipulate logs that come in, you can enrich logs, add/remove fields etc etc..
    - Outputs will forward the logs to an ELastic Index, Logstash instance or Message Queue - we will be doing forwarding logs to a logstash instance that on its turn forwards all students logs to a RabbitMQ server.

> The order of these scripts is important, they will be processed in order!

INPUTS
====
For the lab, the set up is as follows:

Windows clients (***winlogbeat agent***) --push-to--> central Logstash --push-to--> `central RabbitMQ`  

Student Kali Linux machines with ELK (***logstash***) --pull-from--> `central RabbitMQ` 

So this means your client machines will be forwarding their logs to a central Logstash server (managed and configure by the instructors), so you will not be forwarding your logs to your __own__ logstash. You logstash ***INPUT*** will fetch all the students logs from the RabbitMQ server. 

> IMPORTANT :  
> Change the __password__ in the config file below with the ***PROVIDED_PASSWORD***
> Change the __queue__ to your student number : for example RabbitQueue_StudentXX -> ***RabbitQueue_Student02***

```yml
input {
    rabbitmq {
        host => "10.0.0.6"
        port => 5672
        user => "thadmin"
        password => "PROVIDED_PASSWORD"
        queue => "RabbitQueue_StudentXX"
        passive => true
        exclusive => false
        durable => true
        auto_delete => false
        subscription_retry_interval_seconds => 3
        threads => 2
	prefetch_count => 256
    }
}
```

FILTERS
====




OUTPUTS
====

The examle below is using a ___tag___ that we defined in our ***winlogbeat agent***, this can be done in the logstash filters as well. We will configure the logging agents in the next chapter, just keep this in mind. The index name is derived from metadata provided by the winlogbeat agent as well

```yml
output {
  if "windows" in [tags] {
    elasticsearch {
      hosts => "es01:9200"
      index => "%{[@metadata][beat]}-%{[@metadata][version]}"
      #index => "winlogbeat-%{+YYYY.MM.dd}"
    }
  }
}
```






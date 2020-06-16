#   Chapter 1.8.2 - Logstash FIlters

>This chapter explains how to query the 2 previously API's and add custom fields with the results to the logs, before they get indexed>

1.0 LOGSTASH configuration for FreqServer on DNS records
===

```code
cd /opt/threathunt/logstash/logstash_enrich
sudo nano 410_enrich_filter_windows_sysmon_dns_freq.conf
```

```yaml
filter {                                                                                                                        
    if [winlog][event_id] == 22 {                                                                                                
        mutate {                                                                                                                 
            add_tag => [ "DNS_log" ]                                                                                              
            add_field => { "[QueryName]" => "%{[dns][question][name]}" }
        }
        tld {
          source => "QueryName"
        }
        # Rename fields from the tld filter plugin
        mutate {
          rename => { "[tld][domain]" => "ENRICH_DNS_highest_registered_domain" }
          rename => { "[tld][subdomain]" => "ENRICH_DNS_tld_sub" }
          rename => { "[tld][trd]" => "ENRICH_DNS_subdomain" }
          rename => { "[tld][tld]" => "ENRICH_DNS_top_level_domain" }
          rename => { "[tld][sld]" => "ENRICH_DNS_parent_domain" }
        }
        # Check if ENRICH_DNS_tld_sub has any value, to avoid REST API errors -> then it has a value then query for frequency score
        if [ENRICH_DNS_tld_sub] {
          rest {
            request => {
              url => "http://freqserver:10004/measure1/%{ENRICH_DNS_tld_sub}"
            }
            sprintf => true
            json => false
            target => "ENRICH_DNS_FullQuery_FREQ"
          }
        }
        # Check if ENRICH_DNS_subdomain has any value, to avoid REST API errors -> then it has a value then query for frequency score
        if [ENRICH_DNS_subdomain] {
          rest {
            request => {
              url => "http://freqserver:10004/measure1/%{ENRICH_DNS_subdomain}"
            }
            sprintf => true
            json => false
            target => "ENRICH_DNS_SubQuery_FREQ"
          }
        }
        # Check if ENRICH_DNS_parent_domain has any value, to avoid REST API errors -> then it has a value then query for frequency score
        if [ENRICH_DNS_parent_domain] {
          rest {
            request => {
              url => "http://freqserver:10004/measure1/%{ENRICH_DNS_parent_domain}"
            }
            sprintf => true
            json => false
            target => "ENRICH_DNS_ParentQuery_FREQ"
          }
          mutate {
          convert => [ "ENRICH_DNS_ParentQuery_FREQ", "float" ]
          }
        }
        # If parent domain has a low frequency score, tag it with "DGA" -> scores below 4.0 are potential DGA's
        if [ENRICH_DNS_ParentQuery_FREQ] and [ENRICH_DNS_ParentQuery_FREQ] != 0 and [ENRICH_DNS_ParentQuery_FREQ] < 4.0 {
          mutate {
            add_tag => [ "DGA" ]
          }
        }
        rest {
          request => {
            url => "http://domainstats:20000/alexa/%{QueryName}"
          }
          sprintf => true
          json => false
          target => "ENRICH_domain_score"
        }
        mutate {
          convert => [ "ENRICH_domain_score", "float" ]
        }
        # If domain score value exists, tag it with "top1-m"
        if [ENRICH_domain_score] >= 1.0 {
          mutate {
            add_tag => [ "top-1m" ]
          }
        }
        rest {
          request => {
            url => "http://domainstats:20000/domain/creation_date/%{QueryName}"
          }
          sprintf => true
          json => false
          target => "ENRICH_domain_creationdate"
        }
        mutate {
          remove_field => [ "QueryName" ]
        }
    }
}
```

```code
cd /opt/threathunt/logstash/logstash_enrich
ls -l
cp 410_enrich_filter_windows_sysmon_dns_freq.conf /opt/threathunt/logstash/pipeline
```

```code
sudo docker restart logstash_rest
sudo docker container logs logstash_rest --follow
```
You should this as the last messages in your logstash container logs, this means Logstash has rebooted and succesfully applied the new configuration:

```
[2020-06-16T10:37:22,694][INFO ][logstash.inputs.beats    ][main] Beats inputs: Starting input listener {:address=>"0.0.0.0:5044"}
[2020-06-16T10:37:22,719][INFO ][logstash.javapipeline    ][main] Pipeline started {"pipeline.id"=>"main"}
[2020-06-16T10:37:22,849][INFO ][logstash.agent           ] Pipelines running {:count=>1, :running_pipelines=>[:main], :non_running_pipelines=>[]}
[2020-06-16T10:37:22,874][INFO ][org.logstash.beats.Server][main] Starting server on port: 5044
[2020-06-16T10:37:23,092][INFO ][logstash.agent           ] Successfully started Logstash API endpoint {:port=>9600}
```
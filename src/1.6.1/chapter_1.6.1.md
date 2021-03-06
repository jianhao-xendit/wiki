#   Chapter 1.6.1 - API's : Domain Stats / FreqServer

>This chapter explains how to enrich certain logs by adding Logstash filters on your `Kali Linux Machine`, we'll be using tagging and extra fields for frequency scores (how random is a certain domain/service/file name?), Adding geo-location fields, add tags to define if a device is internal/external and pull in whois information on dns queries.


1.0 INSTALLING FreqServer
===
_"Mark Baggett’s (@MarkBaggett - GSE #15, SANS SEC573 Author) Awesome-Sauce tool for detecting randomness using NLP techniques rather than pure entropy calculations. Uses character pair frequency analysis to determine the likelihood of tested strings of characters occurring based upon the chosen frequency tables (some prebuilt English text freq tables provided). Extremely useful for detecting high entropy where it shouldn’t be. Especially powerful for discovering DNS based DGAs commonly used for malware C2 and exfiltration. Think bigger than DGAs though. Random file names, script names, process names, service names, workstation names, TLS certificate subjects and issuer subjects, etc."_

First we're going to install __FreqServer__ on our `Kali Lunix machine`

- GITHUB : ***[Original Github Code](https://github.com/MarkBaggett/freq)*** (we're using the latest docker build from Security Onion)
- DOCUMENTATION : ***[Security Onion FreqServer](https://github.com/Security-Onion-Solutions/security-onion/wiki/FreqServer)***
- YOUTUBE : ***[Getting the Most out of Freq and Domain_Stats by Mark Baggett](https://www.youtube.com/watch?v=dfrh1FaFUic&feature=youtu.be)***

```code
cd /opt/threathunt/docker-compose
sudo docker-compose -f docker-compose.freqserver.yml up -d
```

Let's have a closer look at the docker-compose file:

```yaml
version: '3'
services:    
  freqserver:
    image: securityonionsolutions/so-freqserver
    container_name: freqserver
    restart: unless-stopped
    ports:
      - 10004:10004
    networks:
      - elastic
networks:
  elastic:
    driver: bridge
```

We're now running freqserver (a python script in a docker container, listening on port 10004).

Let's feed is a valid english word:

``` code
curl http://localhost:10004/measure1/trainstation
```

![Screenshot command](./assets/01-freq_trainstation.jpg)

We get a score of 8.7832 - the idea here is that the higher the score, the more likely this is an existing (and thus not randomly generated term).

Now let's try some random DGA (the one from [Wannacry](https://www.fireeye.com/blog/threat-research/2017/05/wannacry-malware-profile.html) in this case)

```code
curl http://localhost:10004/measure1/www.iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com
```

We get a score of 3.5319 - a much lower score, as this is clearly not a human generated FQDN. So the lower the score, the more "risky" this is. Now of course there are valid tools that generated random hostnames/ fqdns / servic names and so on - but we'll enriching logs here in multiple ways to create more context.

- Have we seen this domain/service name/file name before in our evironment?
- How often, how rare is this?  
- What's the frequency score?  
- what was the parent process that made the call to the domain or created the service?  
- If it is a domain name, what is the geo-location?
- When was the domain registered? Is it in the Cisco Umbrella Top1 most visited websites?

![Screenshot command](./assets/01-freq_dga.jpg)

2.0 INSTALLING Domain_Stats
===

Next let's install __Domain_Stats__ on our `Kali Lunix machine`

- GITHUB : ***[Original Github Code](https://github.com/MarkBaggett/domain_stats)*** (we're using the latest docker build from Security Onion)
- DOCUMENTATION : ***[Security Onion FreqServer](https://github.com/Security-Onion-Solutions/security-onion/wiki/FreqServer)***
- YOUTUBE : ***[Getting the Most out of Freq and Domain_Stats by Mark Baggett](https://www.youtube.com/watch?v=dfrh1FaFUic&feature=youtu.be)***

```code
cd /opt/threathunt/docker-compose
sudo docker-compose -f docker-compose.domainstats.yml up -d
```

Let's have a closer look at the docker-compose file:

```yaml
version: '3'
services:  
  domainstats:
    image: securityonionsolutions/so-domainstats:latest
    container_name: domainstats
    restart: unless-stopped
    ports:
      - 20000:20000
    volumes:
      - /opt/threathunt/domain_stats/top-1m.csv:/opt/domain_stats/top-1m.csv
    networks:
      - elastic
networks:
  elastic:
    driver: bridge
```

Let's check the creation date of the google.com domain:

```code
curl http://localhost:20000/domain/creation_date/google.com
```

And let's see what position google.com has in the Umbrella Top 1M sites:

```code
curl http://localhost:20000/domain/alexa/google.com
```

What we're doing here is checking a CSV file that is found under /opt/threathunt/domain_stats - so see if the domain is in the Top 1 Million most visited sites, and how high they up the list the domain is. If the domain is not found in this list, it's potentially suspicious - combining that with other contextual information this helps an analist to quickly triage noise from real suspicious events.

You can update the list by running:

```code
cd /opt/threathunt/domain_stats
sudo /UmbrellaTop1M.sh 
```

You can schedule this if you want, and the screenshot below shows how __dell.com__ has moved from position 2061 to 1914 with the latest update. This is not really that important, we're mainly interested in fqdn that are __NOT__ in the list at all.

![Screenshot command](./assets/02-domainstats-update.jpg)

internal/external tagging  
geolocation  
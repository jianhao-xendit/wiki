#   Chapter 1.6.2.1 - Configuring Logstash to use SSL

On your `Kali Linux Machine`

dependencies:  
apt-get update  
apt-get install openssl  

Or is openssl is already installed:  

apt-get update  
sudo apt-get --only-upgrade install openssl  

```code
mkdir -p /opt/threathunt/logstash/pki/
cd /opt/threathunt/logstash/pki/
sudo openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout /opt/threathunt/logstash/pki/logstash.key -out /opt/threathunt/logstash/pki/logstash.crt
```
You will have to fill out the requested information as seen below:  

>Country Name (2 letter code) [AU]:**BE**  
State or Province Name (full name) [Some-State]:**Antwerp**  
Locality Name (eg, city) []:**Antwerp**  
Organization Name (eg, company) [Internet Widgits Pty Ltd]:**CrimsonCORE**  
Organizational Unit Name (eg, section) []:**Threat Hunting Department**  
Common Name (e.g. server FQDN or YOUR name) []:`ENTER_YOUR_LOGSTASH_IP_HERE`  
Email Address []:**luks@crimsoncore.be**  

then go to your logstash pipeline directory:

```code
cd /opt/threathunt/logstash/pipeline/
nano 001_Beats-SSL-input.conf
```

and add the following - notice we're using a different port than the standard 5044, this way we can have multiple inputs for testing.

```yaml
input {
  beats {
    port => 5045
    ssl => true
    ssl_certificate => "/usr/share/logstash/pki/logstash.crt"
    ssl_key => "/usr/share/logstash/pki/logstash.key"
  }
}
```
then restart you logstash container:
```code
docker container restart logstashrest
```

start updog in the PKI directory to host the certs  

ON YOUR WINDOWS machine
surf to the folder and download the logstasg.crt

http://yourkalimachine:9090

copy the file to "C:\program files\winlogbeat-7.6.2-windows-x86_64"

and update the config to reflect the following:

```yaml
output.logstash:
  hosts: ["172.16.100.157:5045"]
 # ssl.certificate: "C:/logstash.crt"
  ssl.certificate_authorities: ["C:/logstash.crt"]
 # ssl.key: "c:/logstash.key"
  ssl.verification_mode: none
```

THIS IS THE WAY: https://github.com/Busindre/How-to-configure-SSL-for-FileBeat-and-Logstash-step-by-step

https://burnhamforensics.com/2019/02/25/elk-beats-securing-communication-with-logstash-by-using-ssl/


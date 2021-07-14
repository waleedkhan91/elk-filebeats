# Filebeats

Installing Filebeats on Reverse proxy (Staging)

```
sudo vi /etc/yum.repos.d/elastic.repo

[elasticsearch-7.x]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```
```
sudo vi /etx/yum.conf

proxy=http://xxx.xxx.xxx.xxx:3128
```
The proxy is different for each organization.
```
sudo yum list filebeat --show-duplicates --disablerepo=* --enablerepo=elasticsearch*
sudo yum install filebeat-7.3.1-1 --disablerepo=* --enablerepo=elasticsearch*
sudo systemctl enable filebeat
```

To start filebeat following configuration is applied. only changes are mentioned here. rest is default.


```
sudo vi /etc/filebeat/filebeat.yml
```
```

#=========================== Filebeat inputs =============================

filebeat.inputs:

# Each - is an input. Most options can be set at the input level, so
# you can use different inputs for various configurations.
# Below are the input specific configurations.

- type: log

  # Change to true to enable this input configuration.
  enabled: true

  # Paths that should be crawled and fetched. Glob based paths.
  paths:
    #- /var/log/*.log
    #- c:\programdata\elasticsearch\logs\*
    - /var/nginxlog/theomancy/access.log

#----------------------------- Logstash output --------------------------------
output.logstash:
  enabled: true
  # The Logstash hosts
  #hosts: ["localhost:5044"]
  hosts: ["10.13.xx.xx:30443","10.13.xx.xx:30443"]
#================================ Logging =====================================

# Sets log level. The default log level is info.
# Available log levels are: error, warning, info, debug
logging.level: error
logging.to_files: true
logging.files:
  path: /var/filebeatlogs
  name: filebeat
  keepfiles: 7
  permissions: 0644
```
# Elasticsearch-Curator

# Installation
add repo in /etc/yum.repos.d/curator.repo and install


```
[curator-5]
name=CentOS/RHEL 7 repository for Elasticsearch Curator 5.x packages
baseurl=https://packages.elastic.co/curator/5/centos/7
gpgcheck=0
gpgkey=https://packages.elastic.co/GPG-KEY-elasticsearch
enabled=1

sudo yum install elasticsearch-curator (this has its own repo not elastic repo)
```
It is installed in /opt/elasticsearch-curator. Default config is in ~/.curator/curator.yml but change the location and give configin command as --config.


# Config

sudo mkdir /home/Zunera.nayyar/.curator
cd .curator
sudo curator.yml


#add following config
```
client:
  hosts:
    - 0.0.0.0
  port: 9200
  url_prefix:
  use_ssl: False
  certificate:
  client_cert:
  client_key:
  ssl_no_validate: False
  http_auth:
  timeout: 30
  master_only: False

logging:
  loglevel: INFO
  logfile: '/home/zunera.nayyar/.curator/log.log'
  logformat: default
  blacklist: ['elasticsearch', 'urllib3']
```

# Curator_CLI
```
sudo curator_cli --config /home/zunera.nayyar/.curator/curator.yml show-indices

sudo curator_cli --config /home/zunera.nayyar/.curator/curator.yml show-indices --filter_list '[{"filtertype":"age","source":"creation_date","direction":"older","unit":"days","unit_count":5},{"filtertype":"pattern","kind":"prefix","value":"nginx-rp-"}]'
  
sudo curator_cli --config /home/zunera.nayyar/.curator/curator.yml delete-indices --filter_list '[{"filtertype":"age","source":"creation_date","direction":"older","unit":"days","unit_count":5},{"filtertype":"pattern","kind":"prefix","value":"nginx-rp-"}]'
```


# Curator Actions File

```
actions:
  1:
    action: delete_indices
    description: >-
      Delete indices older than 7 days (based on index name), for filebeat-
      prefixed indices. Ignore the error if the filter does not result in an
      actionable list of indices (ignore_empty_list) and exit cleanly.
    options:
      ignore_empty_list: True
      disable_action: False
    filters:
    - filtertype: pattern
      kind: prefix
      value: nginx-
    - filtertype: age
      source: name
      direction: older
      timestring: '%Y.%m.%d'
      unit: days
      unit_count: 5
  2:
    action: delete_indices
    description: >-
      Delete indices older than 7 days (based on index name), for filebeat-
      prefixed indices. Ignore the error if the filter does not result in an
      actionable list of indices (ignore_empty_list) and exit cleanly.
    options:
      ignore_empty_list: True
      disable_action: False
    filters:
    - filtertype: pattern
      kind: prefix
      value: nginx-
    - filtertype: age
      source: name
      direction: older
      timestring: '%Y.%m.%d'
      unit: days
      unit_count: 5
```


# Curator
```

sudo curator --config /home/zunera.nayyar/.curator/curator.yml --dry-run /home/zunera.nayyar/.curator/action.yml

####Execution File#######


sudo vi /home/Zunera.nayyar/.curator/curator.sh

#add following lines and save
#!/bin/sh
/opt/elasticsearch-curator/curator --config /home/zunera.nayyar/.curator/curator.yml --dry-run /home/zunera.nayyar/.curator/action.yml
```


# Cron
```
0 17 * * * /bin/bash /home/zunera.nayyar/.curator/curator.sh
```


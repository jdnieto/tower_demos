# ELK Demo
This Playbook rolls out a five node ELK Setup:

 - One Host with Logstash that connects to Twitter and grabs tweets
 - One Host with Kibana, that also acts as Elasticsearch Master
 - Three Elasticsearch Hosts

 ### What do you need for this Demo

  - Five subscribed RHEL 7 or Centos 7 Nodes
  - Your own Twitter API Key/Secret/Token to be placed in _logstash_vars.yml_
  - Modify _elastic_config.j2_ and _logstash.j2_ with your IP adresses
  - Set the variable _{{ tw_keyword }}_ in the Ansible Playbook or Tower with the Twitter Keyword that you want to index

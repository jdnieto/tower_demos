# ELK Demo on Google Cloud
This Playbook rolls out a five node ELK Setup on GCE:

 - One Host with Logstash that connects to Twitter and grabs tweets
 - One Host with Kibana, that also acts as Elasticsearch Master
 - Three Elasticsearch Hosts

 ### What do you need for this Demo

  - A RHEL 7 or Centos 7 Template VM on GCE
  - Your own Twitter API Key/Secret/Token to be placed in _logstash_vars.yml_
  - Modify elastic_config.j2 and logstash.j2 with your IP adresses (they are hardcoded for an empty GCE project )
  - Set the variable _{{ tw_keyword }}_ in the Ansible Playbook or Tower with the Twitter Keyword that you want to index
  - Your GCE credentials in Ansible Tower
  - Your Key inside Tower that allows the Tower VM to log into the GCE-VMs as root

### Running

 - Set the Variables for the gce.yml playbook like this:

```
 ---
gce_instance: logstash,kibana,elastic1,elastic2,elastic3
gce_zone: us-central1-a
gce_machine: n1-highmem-2
gce_state: present
gce_image: (The name of your RHEL 7/Centos7 Template VM goes here)
```

 - Fire the gce.yml Playbook to create the VMs on GCE
 - Open Port 5601 on the Kibana VM
 - Refresh the GCE inventory
 - Fire the _elastic_prepare.yml_ Playbook with the GCE Inventory to roll out the ELK installation

Once the ELK Stack is up, log into the kibana Machine and set the Number of Copies for you index with:

```
 curl -XPUT 10.128.0.3:9200/twitter_elastic_example/_settings -H 'Content-Type: application/json' -d '{"index" : {"number_of_replicas" : 2}}'
```

This has to be done __after__ the first Data Sets have been uploaded by Logstash.

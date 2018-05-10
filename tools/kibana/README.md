# Installing kibana

Simple:

 1. Make sure you get the same version as the version of ElasticSearch you are working with.
 2. Download from: https://artifacts.elastic.co/downloads/kibana/kibana-6.2.2-linux-x86_64.tar.gz
 3. Gunzip and untar to: `/usr/local/kibana/`
 4. Enter directory: `kibana-6.2.2-linux-x86_64`
 5. Update config file (`config/kibana.yml`) to point to desired ElasticSearch host:
     `elasticsearch.url: "http://jasmin-es-test.ceda.ac.uk:9200"`
 6. Start the server: `bin/kibana`
 7. View results at: http://localhost:5601/app/kibana#/home?_g=()
 
## Setting up kibana to be accessed from remote hosts

Make a tweak to the config file:
 `server.host: "jasmin-es-test.ceda.ac.uk"`
 
And restart, and view results at:
 http://jasmin-es-test.ceda.ac.uk:5601/app/kibana#/home?_g=()

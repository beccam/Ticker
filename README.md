# Dependencies
    apt-get install python-pip
    apt-get install python-dev
    apt-get install build-essential

    pip install flask
    pip install blist
    pip install cassandra-driver

# Configuration

* web-python/application.cfg

    DSE_CLUSTER=IP_ADDRESS_OF_NODE
    DSE_SOLR_DC=NAME_OF_SOLR_DC

* seeding/seed.py
    
    ip_addresses = 'IP_ADDRESS_OF_NODE'
    
* seeding/stream.py
    
    ip_addresses = 'IP_ADDRESS_OF_NODE'

* Configure Replication
    CREATE KEYSPACE ticker WITH replication = {
      'class': 'NetworkTopologyStrategy',
      'NAME_OF_DC': '1',
    };

*  Setup Schema

    cqlsh -f cql/ticker.cql
    
* Setup Solr Core
    
    dsetool -h IP_ADDRESS_SOLR create_core ticker.latest generateResources=true
    
# Begin streaming data    
    
* Seed Data

    cd seeding
    chmod +x seed.py
    python seed.py
    
* Stream Data
    
    cd seeding
    chmod +x stream.py
    nohup python stream.py &
    
# Start the Application

    ./web-python/run
    
# Navigate to the Web Portal

    http://localhost:5000

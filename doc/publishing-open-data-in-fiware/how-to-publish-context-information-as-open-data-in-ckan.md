Through the mechanisms described in the section [Development of context-aware
applications](/development-context-aware-applications/introduction/),
the Context Broker can be used to publish and consume context
information. In particular, this information can be indeed open data and
consumed through the queries and subscriptions APIs (NGSI10). This way,
it is possible to publish real time or dynamic data, typically well
structured, and offered it as open data through the reutilization by
developers. For instance, it is possible to offer in real time data from
sensors or systems to leverage the creation of new applications.

However, the Context Broker only provides the latest snapshot of the
context information in a given moment, but there are many cases where it
is also required to store and publish historical information of the
context data generated over time. This is one of the usages of the Open
Data publication GE, CKAN, in FIWARE.

CKAN is an open source solution for the publication, management and
consumption of open data, usually, but not only, through static
datasets. CKAN allows to catalogue, upload and manage open datasets and
data sources, while supports searching, browsing, visualizing or
accessing open data. CKAN is the Open Data publication platform that is
most widely used by many cities, public authorities and organizations.

You may take advantage of the connectors supported by the Context Broker
that automatically generate historic records generated each time there
is a change in the context information and make those records available
for upload on the Open Data publication GE. The data is then stored in a
Datastore, and can be downloaded and queried through REST APIs.

In order to achieve this behaviour it is necessary to deploy and
configure Cygnus, a piece of software complementary to the Context
Broker GE. The instructions to install Cygnus can be found
[here](https://fiware-cygnus.readthedocs.io/en/latest/index.html). 

Once Cygnus has been installed, it is required to configure it. In a
nutshell, there are three steps: configure CKAN storage, create the
desired subscriptions in the Context Broker and run the process.

This sink persists the data in a datastore in CKAN. Datastores are
associated to CKAN resources and as CKAN resources we use the
entityId-entityType string concatenation. All CKAN resource IDs belong
to the same dataset (also referred as package in CKAN terms), which name
is specified with the default\_dataset property (prefixed by
organization name) in the CKAN sink configuration.

In order to configure CKAN storage, the file cygnus.conf has to be
edited, specifying the CKAN sink, the sink channel (where to read the
notifications from), the CKAN’s user API key, CKAN instance detail (IP,
port, etc.), and Context Broker instance endpoint. All the details can
be found at:

https://github.com/telefonicaid/fiware-connectors/blob/master/flume/README.md

Once the storage has been configured, it is required to run the process
with, for instance, the following command:

    $ APACHE_FLUME_HOME/bin/flume-ng agent –conf APACHE_FLUME_HOME/conf -f 
    APACHE_FLUME_HOME/conf/cygnus.conf -n cygnusagent -Dflume.root.logger=INFO,console

Once the connector is running, it is necessary to tell Orion Context
Broker about it, in order Orion can send context data notifications to
the connector. This can be done on behalf of the connector by performing
the following curl command (specifying the endpoint where Cygnus is
listening):

    (curl localhost:1026/v1/subscribeContext -s -S –header 'Content-Type: application/json' –header 'Accept: application/json' -d @- | python -mjson.tool) <<EOF
    {
        "entities": [
            {
                "type": "Room",
                "isPattern": "false",
                "id": "Room1"
            }
        ],
        "attributes": [
            "temperature"
        ],
        "reference": "http://host_running_cygnus:5050/notify",
        "duration": "P1M",
        "notifyConditions": [
            {
                "type": "ONCHANGE",
                "condValues": [
                    "pressure"
                ]
            }
        ],
        "throttling": "PT5S"
    }
    EOF

Once the process starts storing data, the dataset and resource will
appear in CKAN and it will be possible to browse and download data from
CKAN portal, or query it throught he Datastore API. More information at:

http://docs.ckan.org/en/ckan-2.2/datastore.html\#the-datastore-api 

For instance, the following query would return the first 5 results of a
dataset.  
 GET
CKAN\_HOST/api/action/datastore\_search?resource\_id=5a2ed5ca-2024-48d7-b198-cf9d95c7374d&limit=5

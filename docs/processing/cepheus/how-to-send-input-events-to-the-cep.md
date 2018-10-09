The CEP provides a REST service for receiving input events. Event has a
list of attributes, each attribute as a name and a value, and the name
of the event is given in the "Name" attribute. The CEP supports three
formats for input events: tag-delimited, JSON and NGSI-XML.

To send event in the JSON format use the following query:

    POST <cep_host>:<port>/{instance_name}/rest/events 
    //(Example: POST:  http://130.206.81.23:8080/ProtonOnWebServer/rest/events)
    Headers: {'Content-Type’: 'application/json’; 'X-Auth-Token' : <Oauth2.0 TOKEN>}
    {  "Name":"TrafficReport", 
       "volume":"1000" 
    }

To send the same event in the tag-delimited format use the same query
but with a different content type and a different data format:

    POST <cep_host>:<port>/{instance_name}/rest/events 
    //(Example: POST:  http://130.206.81.23:8080/ProtonOnWebServer/rest/events)
    Headers: {'Content-Type’: ‘text/plain’; 'X-Auth-Token' : <Oauth2.0 TOKEN>}
    Name=TrafficReport;volume=1000;

The NGSI-XML format allows the CEP to get input events from the Context
Broker GE. If you want the CEP to get input events from the Context
Broker GE, you should add a subscription to the Context Broker with the
CEP URL given above.

The first thing you need to do is to connect your IoT devices or
Gateways to the above described scenario. This basically means that your
IoT devices’ observations reach a ContextBroker.

In the following paragraphs, we provide an example on how you would
connect your devices using Ultralight2.0 (UL2.0) - which is a proposed
simplification of the SensorML (SML) standard - and get those devices
sending  their measurements (observations) to the FIWARE Lab public
ContextBroker. Ultralight2.0 is selected in this example because of its
simplicity. 

If you want to quickly connect or simulate virtual devices you may also
check FIGWAY, a set of simple python scripts working as a client SDK for
any desktop PC, laptop or gateway supporting a python2.7 environment.
This way you may skip the steps described below and use the python
commands as described in the README.md file available at the Github
repository linked above.

You can find a good summary of the APIs described below also at:  

[http://docs.telefonicaiotiotagents.apiary.io/\#reference/undefined/device/remove-a-device ](http://docs.telefonicaiotiotagents.apiary.io/#reference/undefined/device/remove-a-device)

Basically, there are four simple steps to follow:

**1. Create an IDAS Service**  
 If you are using the public IDAS instance with the public “OpenIoT”
testing service available at 130.206.80.47 (Port 5073) you may skip this
step. Just keep in mind the shared secret for this public service (that
your devices need to know) is the string “4jggokgpepnvsb2uv4s40d59ov”.  
 Otherwise, creating an IDAS Service is basically a simple HTTP POST
like this:

    POST <idas_host>:<idas_port>/iot/services
    //Example: HTTP POST: http://130.206.80.40:5371/iot/services 
    Headers: {'content-type': 'application/json’; 'X-Auth-Token' : [TOKEN]; "Fiware-Service: OpenIoT”; "Fiware-ServicePath: /"}
    Payload:
    {
      "services": [
        {
        "apikey": "4jggokgpepnvsb2uv4s40d59ov",
        "token": "token2",
        "cbroker": "http://0.0.0.0:1026",
        "entity_type": "thing",
        "resource": "/iot/d"
        }
      ]
    }

Where 0.0.0.0:1026 might be replaced by a private instance of
ContextBroker or just leave it like this (0.0.0.0:1026) if the public
instance at the same VM (130.206.80.40:1026) will be used. The apikey
string should be updated with a shared secret to be known by devices.

**2.    Register your IoT device**  
 Before your device sends observations or receives commands a register
operation is needed:

    POST <idas_host>:<idas_port>/iot/devices
    //Example: http://130.206.80.40:5371/iot/devices 
    Headers: {'content-type': 'application/json’; 'X-Auth-Token' : [TOKEN]; "Fiware-Service: OpenIoT”; "Fiware-ServicePath: /"}
    Payload:
    {"devices": [
        { "device_id": ”[DEV_ID]",
        "entity_name": ”[ENTITY_ID]",
        "entity_type": "thing",
        "timezone": ”Europe/Madrid",
    "attributes": [
            { "object_id": "t",
            "name": "temperature",
            "type": "int"
            } ],
     "static_attributes": [
            { "name": "att_name",
            "type": "string",
            "value": "value"
            }]}]}

The important parameters to be defined are:  
 ●    [DEV\_ID] the device identifier at IDAS.  
 ●    [ENTITY\_ID] the entity ID to be used at the ContextBroker will be
“thing:[ENTITY\_ID]”  
 ●    "attributes" they should include an alias (a letter representing
this attribute).  
 ●    "static\_attributes" only if your device needs to define static
attributes (sent in every observation)

**3.     Send Observations related to your IoT device**  
 Sending an observation from IoT devices is extremely efficient and
simple with the following query:

    POST  <idas_host>: <idas_port>/d?k= <apikey>&i= <device_ID>
    //Example: http://130.206.80.40:5371/iot/d?k=[APIKEY]&i=[DEV_ID]
    Headers: {'content-type': 'application/text’; 'X-Auth-Token' : [TOKEN]; "Fiware-Service: OpenIoT”; "Fiware-ServicePath: /"}
    Payload: ‘ t|25‘

The previous example sends an update of the Temperature attribute that
is automatically sent by IDAS to the corresponding entity at the
ContextBroker and FIWARE Service defined in our IDAS service.

Sending multiple observations in the same message is also possible with
the following payload:

    //“alias1|value1#alias2|value2#alias3|value3...”
    //Example: http://130.206.80.40:5371/iot/d?k=[APIKEY]&i=[DEV_ID]
    Headers: {'content-type': 'application/text’; 'X-Auth-Token' : [TOKEN]; "Fiware-Service: OpenIoT”; "Fiware-ServicePath: /"}
    Payload: ‘t|23#h|80#l|95#m|Quiet‘

**4.     Reading measurements sent by your IoT device**  
 Finally, after connecting your IoT devices this way you (or any other
developer with the right access permissions) should be able to use the
ContextBroker NGSI API to read the NGSI entity assigned to your device. 

The Entity ID will be the following “thing:[ENTITY\_ID]”.

In our particular example described above, you may check in the public
ContextBroker (at 130.206.80.40:1026), the
Entity\_ID=”thing:[ENTITY\_ID]” and the attribute “Temperature” with the
correct updated value. For examples on how to access the ContextBroker,
please refer to this component section.

 

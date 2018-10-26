In order to send commands to devices, developers just need to know which
attributes correspond to commands and update them.

IoT integrators need to declare the command related attributes at the registry
process (POST request) in the following way:

    POST <idas_host>:<idas_port>/iot/devices
    //Example: HTTP POST: http://130.206.80.40:5371/iot/devices
    Headers: {'content-type': 'application/json’; 'X-Auth-Token' : [TOKEN]; "Fiware-Service: OpenIoT”; "Fiware-ServicePath: /"}
    Payload:
    {"devices": [
        { "device_id": ”[DEV_ID]",
        "entity_name": ”[ENTITY_ID]",
        "entity_type": "thing",
        "endpoint": "http://[DEVICE_IP]:[PORT]",
        "timezone": ”Europe/Madrid",
        "commands": [
            { "name": ”RawCommand",
            "type": "command",
            "value": “[Dev_ID]@RawCommand|%s"
            } ],
        "attributes": [

Any update on this attribute “RawCommand” at the NGSI entity in the
ContextBroker will send a command to your device.

If the row **"endpoint": "\*\***http://[DEVICE\_IP]:[PORT]"** is declared, then
your device is supposed to be listening for commands at that URL in a
synchronous way.

If that enpoint is not declared (if that row does not exist) then yur devices is
supposed to work in a polling mode and therefore receiving commands in an
asynchronous way (i.e. when the device proactively asks for commands).

For a device working in the polling mode to receive commands, the full pending
queue of commands will be received with the following HTTP GET request:

    POST <idas_host>:<idas_port>/d?k=<apikey>&i=<device_ID>
    //Example: HTTP GET:
    Headers: {'content-type': 'application/text’; 'X-Auth-Token' : [TOKEN]; "Fiware-Service: OpenIoT”; "Fiware-ServicePath: /"}
    http://130.206.80.40:5371/iot/d?k=[APIKEY]&i=[DEV_ID]

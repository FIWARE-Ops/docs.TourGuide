<hr class="iotagents" style="display:none"/>
<h2>Reading Data from IoT Devices</h2>

The first thing you need to do is to connect your IoT devices or Gateways to
FIWARE. This basically means that your IoT devices’ observations reach a Context
Broker.

During this tutorial, we provide an example on how you would connect your
devices using
[Ultralight2.0](https://github.com/telefonicaid/iotagent-ul#protocol) (UL2.0) -
which is a proposed simplification of the SensorML (SML) standard - and get
those devices sending their measurements (observations) to the FIWARE Lab public
Context Broker. Ultralight2.0 is selected in this example because of its
simplicity. In addition we show you how you can do the same locally with the
_FIWARE Tour Guide Application_ as it integrates the
[IDAS UL2.0 IoT Agent](https://github.com/telefonicaid/iotagent-ul).

If you want to quickly connect or simulate virtual devices you may also check
[https://github.com/telefonicaid/fiware-figway](FIGWAY), a set of simple python
scripts working as a client SDK for any desktop PC, laptop or gateway supporting
a Python 2.7 environment. This way you may skip the steps described below and
use the python commands as described in the README.md file available at the
GitHub repository linked above.

For a more detailed description of the IDAS (FIWARE IoT Agent suite) APIs,
please have a look [here](http://docs.telefonicaiotiotagents.apiary.io/).

A typical IoT data workflow using FIWARE consists of the following steps:

Step 1 : **Create an IDAS Service**

If you are using the public IDAS instance with the public `openiot` testing
service available at `130.206.80.47` (Port `5073`) you may skip this step. Just
keep in mind the shared secret for this public service (that your devices need
to know) is the string `4jggokgpepnvsb2uv4s40d59ov`. Otherwise, creating an IDAS
Service consists of a simple HTTP POST like this:

```
POST http://130.206.80.40:5371/iot/services

Headers:

{
  'Content-Type':       'application/json',
  'X-Auth-Token' :      '[TOKEN]',
  'Fiware-Service':     'openiot',
  'Fiware-ServicePath': '/'
}

Payload:

{
  "services": [
    {
      "apikey":      "4jggokgpepnvsb2uv4s40d59ov",
      "cbroker":     "http://0.0.0.0:1026",
      "entity_type": "thing",
      "resource":    "/iot/d"
    }
  ]
}
```

Where `0.0.0.0:1026` might be replaced by a private instance of a Context Broker
or just leave it as it is (`0.0.0.0:1026`) if the public instance is running at
the same VM (`130.206.80.40:1026`) will be used. The apikey string should be
updated with a shared secret to be known by your IoT devices. An OAuth token is
needed as well, a simple way of obtaining one is described
[here](http://fiware-orion.readthedocs.io/en/develop/quick_start_guide/index.html).

Likewise, you may want to experiment using the _FIWARE Tour Guide Application_.
Assuming you have installed it locally, you can issue the following request:

```
POST http://localhost:4041/iot/services/

Headers:

{
  'Content-Type':       'application/json',
  'Fiware-Service':     'tourguide',
  'Fiware-ServicePath': '/'
}

Payload:

{
  "services": [
    {
      "apikey":   "tourguide-devices",
      "cbroker":  "http://orion:1026",
      "resource": "/iot/dev-restaurants"
    }
  ]
}
```

Step 2: **Register your IoT device**

Before your device sends observations or receives commands a register operation
is needed:

```
POST http://130.206.80.40:5371/iot/devices

Headers:

{
  'Content-Type':       'application/json',
  'X-Auth-Token' :      '[TOKEN]',
  'Fiware-Service' :    'openiot',
  'Fiware-ServicePath': '/'
}

Payload:

{
  "devices": [
    {
      "device_id":   "[DEV_ID]",
      "entity_name": "[ENTITY_ID]",
      "entity_type": "thing",
      "timezone":    "Europe/Madrid",
      "attributes": [
        {
          "object_id": "t",
          "name":      "temperature",
          "type":      "number"
        }
      ],
      "static_attributes": [
        {
          "name":  "attr_name",
          "type":  "string",
          "value": "value"
        }
      ]
    }
  ]
}
```

The important parameters to be provided are:

-   `entity_type` the entity type to be used at the Context Broker. In the
    example above, `thing`.
-   `entity_name` the entity ID to be used at the Context Broker. In the example
    above, `thing:[ENTITY\_ID]`.
-   `attributes` they should include an alias (a letter representing this
    attribute).
-   `static_attributes` only if your device needs to define static attributes
    (sent in every observation).

Likewise, using the _Tour Guide Application_, you can create a device bound to a
restaurant entity. Such device will provide ambient measurements, for instance
`temperature`, for a specific restaurant.

```
POST http://localhost:4041/iot/devices/

Headers:

{
  'Content-Type':       'application/json',
  'Fiware-Service':     'tourguide',
  'Fiware-ServicePath': '/'
}

Payload:

{
  "devices": [
    {
      "device_id": "restaurant-sensor-0115206c51f60b48b77e4c937835795c33bb953f",
      "protocol": "UL20",
      "entity_name": "0115206c51f60b48b77e4c937835795c33bb953f",
      "entity_type": "Restaurant",
      "attributes": [
        {
          "object_id": "t",
          "name":      "temperature",
          "type":      "number"
        }
      ]
    }
  ]
}
```

Step 3: **Send Observations related to your IoT device**

Sending an observation from an IoT device is extremely efficient and simple with
the following HTTP request:

```
POST  http://130.206.80.40:5371/iot/d?k=[APIKEY]&i=[DEV_ID]

Headers:

{
  'Content-Type':  'text/plain',
  'X-Auth-Token' : '[TOKEN]'
}

Payload:

't|25'
```

The previous example sends a new temperature measurement which is automatically
propagated to the corresponding entity at the Context Broker and FIWARE Service
defined previously in our IDAS service. `[API_KEY]` must be the one used when
creating the service and `[DEV_ID]`must be the device identifier formerly
registered.

Similarly, you can do the same with the _Tour Guide Application_:

```
POST http://localhost:7896/iot/d?k=tourguide-devices&i=restaurant-sensor-0115206c51f60b48b77e4c937835795c33bb953f

Headers:

{
  'Content-Type': 'text/plain',
}

Payload:

't|22.3'
```

Sending multiple observations in the same message is also possible with the
following payload:

```
"alias1|value1#alias2|value2#alias3|value3..."

't|23#h|80#l|95#m|Quiet'
```

Step 4: **Reading measurements sent by your IoT device**

Finally, after connecting your IoT devices you (or any other developer with the
right access permissions) should be able to use the Context Broker NGSI API to
query the NGSI entity assigned to your device.

In the first example, using the public FIWARE instance, you may check in the
public Context Broker (at `130.206.80.40:1026`), the
Entity_ID=”thing:[ENTITY\_ID]” and the attribute `temperature` with the correct
updated value.

In the _Tour Guide Application_ example you can check that the temperature value
has been properly propagated by running (with extra headers
`fiware-service: tourguide`):

```
GET http://localhost:1026/v2/entities/0115206c51f60b48b77e4c937835795c33bb953f?attrs=temperature

{
  "id": "0115206c51f60b48b77e4c937835795c33bb953f",
  "type": "Restaurant",
  "temperature": {
    "type": "number",
    "value": "22.3",
    "metadata": {}
  }
}
```

For more examples on how to access the Context Broker, please refer to such
component section.

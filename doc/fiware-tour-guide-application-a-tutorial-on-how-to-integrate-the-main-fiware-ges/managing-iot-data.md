# Managing IoT data
With the TourGuide-App, you can generate, simulate and give persistence to sensors measurements.  For that purpose, we use the information of restaurants, and
generate humidity and temperature sensors for the kitchen and dining room of a restaurant.  Every measurement and information regarding the sensors will be finally propagated to Cygnus, using MySQL database by default.

Note that, to this aim, we will need to previously load the Restaurants information. For further information on how to generate them just execute the load command with the `--help` parameter:

```
./tour-guide load --help
```

And make sure the required services (`tourguide, orion, idas and cygnus`) are up and running.

Once done, we can start working with sensors.

## Working with sensors

In TourGuide-App we provide different ways to work with sensors:

```
$ ./tour-guide sensors
Usage: tour-guide sensors [-h | --help] <command> <options>

Run sensors related commands:

  create                 	Create sensors for the restaurants available in the application.
  update                 	Update all restaurant sensors measurements.
  send-data              	Send a single measurement for a specific sensor.
  simulate-data          	Simulate a sensor sending data over a period of time.

Command options:

  -h  --help             	Show this help.

Use 'tour-guide sensors <command> --help' to get help about a specific <command>.
```

## Creating sensors

Creating sensors is as simple as running:

```
./tour-guide sensors create
```

This will create and initialize four sensors for each of the available restaurants in the application. There will be temperature and relative humidity sensors for both the kitchen and the dining room of the restaurant.  This command executes a script ([sensorsgenerator.js](https://github.com/Fiware/tutorials.TourGuide-App/blob/develop/server/feeders/sensorsgenerator.js)) that uses the IoT agent API to create the sensors and initialize them with a default measurement.

If you want to create the sensors yourself, you can follow this steps:

### Register a new service with the IoT Agent

The first step is to register a new service configuration with the IoT Agent (if it doesn't exists).  To do this we need to send a POST request to `http://localhost:4041/iot/services` with the following JSON payload:
```
{
	"services": [
    	{
        	"apikey": "tourguide-devices",
        	"cbroker": "http://orion:1026",
        	"resource": "/iot/d",
        	"entity_type": "Restaurant"
    	}
	]
}
```
We must include the following HTTP headers when sending the request:

- Fiware-Service, that for TourGuide-App will have a value of `tourguide`,
- Fiware-ServicePath, that will be the organization of the restaurant we want to register sensors for, e.g. `/Franchise1`, as this will allow us to register sensors for restaurants that belong in that organization,
- Content-type of the data we are POSTing, this will be `application/json`.

Now we can do the request:
```
$ (curl -H 'content-type: application/json' -H 'fiware-service: tourguide' -H 'fiware-servicepath: /Franchise1' -X POST 'http://localhost:4041/iot/services' -d @- ) << EOF
{
	"services": [
    	{
        	"apikey": "tourguide-devices",
        	"cbroker": "http://orion:1026",
        	"resource": "/iot/d",
        	"entity_type": "Restaurant"
    	}
	]
}
EOF
```

The response should be `{}` if there was no error and the log of the IoT Agent should have entries like this:
```
time=2016-09-29T11:12:35.920Z | lvl=DEBUG | corr=7c675d9d-e62b-4f67-99f7-e82063ad671a | trans=7c675d9d-e62b-4f67-99f7-e82063ad671a | op=IoTAgentNGSI.RestUtils | srv=tourguide | subsrv=/Franchise1 | msg=Request for path [/iot/services] from [localhost:4041] | comp=IoTAgent
time=2016-09-29T11:12:35.920Z | lvl=DEBUG | corr=7c675d9d-e62b-4f67-99f7-e82063ad671a | trans=7c675d9d-e62b-4f67-99f7-e82063ad671a | op=IoTAgentNGSI.GenericMiddlewares | srv=tourguide | subsrv=/Franchise1 | msg=Body:

{
	"services": [
    	{
        	"apikey": "tourguide-devices",
        	"cbroker": "http://orion:1026",
        	"resource": "/iot/d",
        	"entity_type": "Restaurant"
    	}
	]
}

 | comp=IoTAgent
time=2016-09-29T11:12:35.923Z | lvl=DEBUG | corr=7c675d9d-e62b-4f67-99f7-e82063ad671a | trans=7c675d9d-e62b-4f67-99f7-e82063ad671a | op=IoTAgentNGSI.DeviceGroupService | srv=tourguide | subsrv=/Franchise1 | msg=Creating new set of 1 services | comp=IoTAgent
time=2016-09-29T11:12:35.923Z | lvl=DEBUG | corr=7c675d9d-e62b-4f67-99f7-e82063ad671a | trans=7c675d9d-e62b-4f67-99f7-e82063ad671a | op=IoTAgentNGSI.MongoDBGroupRegister | srv=tourguide | subsrv=/Franchise1 | msg=Looking for entity params ["resource","apikey"] | comp=IoTAgent
Mongoose: mpromise (mongoose's default promise library) is deprecated, plug in your own promise library instead: http://mongoosejs.com/docs/promises.html
time=2016-09-29T11:12:35.930Z | lvl=DEBUG | corr=7c675d9d-e62b-4f67-99f7-e82063ad671a | trans=7c675d9d-e62b-4f67-99f7-e82063ad671a | op=IoTAgentNGSI.MongoDBGroupRegister | srv=tourguide | subsrv=/Franchise1 | msg=Device group for fields [["resource","apikey"]] not found: [{"resource":"/iot/d","apikey":"tourguide-devices"}] | comp=IoTAgent
time=2016-09-29T11:12:35.935Z | lvl=DEBUG | corr=7c675d9d-e62b-4f67-99f7-e82063ad671a | trans=7c675d9d-e62b-4f67-99f7-e82063ad671a | op=IoTAgentNGSI.MongoDBGroupRegister | srv=tourguide | subsrv=/Franchise1 | msg=Storing device group with id [57ecf7235755b6010061870a], type [Restaurant], apikey [tourguide-devices] and resource [/iot/d] | comp=IoTAgent
time=2016-09-29T11:12:35.946Z | lvl=DEBUG | corr=7c675d9d-e62b-4f67-99f7-e82063ad671a | trans=7c675d9d-e62b-4f67-99f7-e82063ad671a | op=IoTAgentNGSI.IOTAMService | srv=tourguide | subsrv=/Franchise1 | msg=response-time: 38 | comp=IoTAgent
```
This information will be stored in a MongoDB database.  We can check this with:
```
$ docker exec -i -t mongodb mongo
MongoDB shell version: 2.6.11
connecting to: test
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
```

We can list the available databases and select the one used by the IoT Agent, `iotagentul`.
```
> show dbs;
admin   	(empty)
iotagentul  0.031GB
local   	0.031GB
orion   	0.031GB
> use iotagentul;
switched to db iotagentul
```
In this database we have two collections, `devices` and `groups`.  The service we've just registered should be in the `groups` collection.
```
> show collections;
devices
groups
system.indexes
> db.groups.find();
{ "_id" : ObjectId("57ecf7235755b6010061870a"), "subservice" : "/Franchise1", "service" : "tourguide", "type" : "Restaurant", "apikey" : "tourguide-devices", "resource" : "/iot/d", "staticAttributes" : [ ], "__v" : 0 }
```
As we can see, the service has been stored.  Next is to register the sensors.

### Register a new sensor with the IoT Agent

To register a new temperature sensor, we need to send a POST request to `http://localhost:4041/iot/devices` with the following JSON payload:
```
{
	"devices": [
    	{
        	"device_id": "DEV_ID",
        	"entity_name": "ENTITY_ID",
        	"protocol": "UL20",
        	"entity_type": "Restaurant",
        	"timezone": "Europe/Madrid",
        	"attributes": [
            	{
                	"object_id": "t",
                	"name": "temperature:kitchen",
                	"type": "Number"
            	}
        	]
    	}
	]
}
```
`ENTITY_ID` will be the Id of the restaurant to which we want to add the sensor, and `DEV_ID` will be the Id of the sensor.  In this example, we'll use the restaurant with Id `0115206c51f60b48b77e4c937835795c33bb953f`.  The Id of the sensor will be a compound of the restaurant Id, the room and the type of the sensor, like `0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature`.

As with the service, we need to add the following HTTP headers in our request:

- Fiware-Service, that for TourGuide-App will have a value of `tourguide`,
- Fiware-ServicePath, that will be the organization of the restaurant, in our example `/Franchise1`,
- Content-type of the data we are POSTing, this will be `application/json`.

We do the request:
```
$ (curl -v -H 'content-type: application/json' -H 'fiware-service: tourguide' -H 'fiware-servicepath: /Franchise1' -X POST 'http://localhost:4041/iot/devices' -d @- ) << EOF
{
	"devices": [
    	{
        	"device_id": "0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature",
        	"entity_name": "0115206c51f60b48b77e4c937835795c33bb953f",
        	"protocol": "UL20",
        	"entity_type": "Restaurant",
        	"timezone": "Europe/Madrid",
        	"attributes": [
            	{
                	"object_id": "t",
                	"name": "temperature:kitchen",
                	"type": "Number"
            	}
        	]
    	}
	]
}
EOF
```
and the response should be `{}` if there was no error.  The log of the IoT Agent should have entries like this:
```
time=2016-09-29T12:55:54.992Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.RestUtils | srv=tourguide | subsrv=/Franchise1 | msg=Request for path [/iot/devices] from [localhost:4041] | comp=IoTAgent
time=2016-09-29T12:55:54.992Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.GenericMiddlewares | srv=tourguide | subsrv=/Franchise1 | msg=Body:

{
	"devices": [
    	{
        	"device_id": "0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature",
        	"entity_name": "0115206c51f60b48b77e4c937835795c33bb953f",
        	"protocol": "UL20",
        	"entity_type": "Restaurant",
        	"timezone": "Europe/Madrid",
        	"attributes": [
            	{
                	"object_id": "t",
                	"name": "temperature:kitchen",
                	"type": "Number"
            	}
        	]
    	}
	]
}

 | comp=IoTAgent
time=2016-09-29T12:55:54.993Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.RestUtils | srv=tourguide | subsrv=/Franchise1 | msg=Handling device provisioning request. | comp=IoTAgent
time=2016-09-29T12:55:54.995Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.MongoDBDeviceRegister | srv=tourguide | subsrv=/Franchise1 | msg=Looking for entity with id [0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature]. | comp=IoTAgent
time=2016-09-29T12:55:54.997Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.MongoDBDeviceRegister | srv=tourguide | subsrv=/Franchise1 | msg=Entity [0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature] not found. | comp=IoTAgent
time=2016-09-29T12:55:54.997Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.MongoDBGroupRegister | srv=tourguide | subsrv=/Franchise1 | msg=Looking for entity params ["service","subservice","type"] | comp=IoTAgent
time=2016-09-29T12:55:54.999Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.MongoDBGroupRegister | srv=tourguide | subsrv=/Franchise1 | msg=Registering device into NGSI Service:
{
	"id": "0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature",
	"type": "Restaurant",
	"name": "0115206c51f60b48b77e4c937835795c33bb953f",
	"service": "tourguide",
	"subservice": "/Franchise1",
	"active": [
    	{
        	"object_id": "t",
        	"name": "temperature:kitchen",
        	"type": "Number"
    	}
	],
	"staticAttributes": [],
	"lazy": [],
	"commands": [],
	"timezone": "Europe/Madrid",
	"protocol": "UL20",
	"internalId": null,
	"subscriptions": []
} | comp=IoTAgent
time=2016-09-29T12:55:55.000Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.DeviceService | srv=tourguide | subsrv=/Franchise1 | msg=No Context Provider registrations found for unregister | comp=IoTAgent
time=2016-09-29T12:55:55.109Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.NGSIParser | srv=tourguide | subsrv=/Franchise1 | msg=Initial entity created successfully. | comp=IoTAgent
time=2016-09-29T12:55:55.110Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.MongoDBDeviceRegister | srv=tourguide | subsrv=/Franchise1 | msg=Storing device with id [0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature] and type [Restaurant] | comp=IoTAgent
time=2016-09-29T12:55:55.120Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.NGSIParser | srv=tourguide | subsrv=/Franchise1 | msg=Device provisioning request succeeded | comp=IoTAgent
time=2016-09-29T12:55:55.121Z | lvl=DEBUG | corr=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | trans=3a59f5cc-3e0f-4d4b-9249-04145eeb3fe3 | op=IoTAgentNGSI.NGSIParser | srv=tourguide | subsrv=/Franchise1 | msg=response-time: 130 | comp=IoTAgent

```

We can check the mongo database again, this time using the `devices` collection:
```
$ docker exec -i -t mongodb mongo
MongoDB shell version: 2.6.11
connecting to: test
> use iotagentul;
switched to db iotagentul
> db.devices.find();
{ "_id" : ObjectId("57ed0f5b5755b6010061870b"), "protocol" : "UL20", "internalId" : null, "subservice" : "/Franchise1", "service" : "tourguide", "name" : "0115206c51f60b48b77e4c937835795c33bb953f", "type" : "Restaurant", "id" : "0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature", "creationDate" : ISODate("2016-09-29T12:55:55.109Z"), "subscriptions" : [ ], "active" : [ { "type" : "Number", "name" : "temperature:kitchen", "object_id" : "t" } ], "__v" : 0 }
```

As we can see, the device has been registered with the IoT Agent.  There is one more check we can do, as we are adding the sensor to an existing entity in Orion.  We can do a request to orion for the restaurant and see if the new attribute has been added:
```
$ curl -s -H 'Fiware-Service: tourguide' http://localhost:1026/v2/entities/0115206c51f60b48b77e4c937835795c33bb953f | json_reformat
{
	"id": "0115206c51f60b48b77e4c937835795c33bb953f",
	"type": "Restaurant",
	"address": {
    	"type": "PostalAddress",
    	"value": {
        	"streetAddress": "Cuesta de las Cabras Aldapa 2",
        	"addressRegion": "Araba",
        	"addressLocality": "Alegría-Dulantzi",
        	"postalCode": "01240"
    	},
    	"metadata": {

    	}
	},
	"aggregateRating": {
    	"type": "AggregateRating",
    	"value": {
        	"ratingValue": 3,
        	"reviewCount": 64
    	},
    	"metadata": {

    	}
	},
	"capacity": {
    	"type": "PropertyValue",
    	"value": 200,
    	"metadata": {

    	}
	},
	"department": {
    	"type": "Text",
    	"value": "Franchise1",
    	"metadata": {

    	}
	},
	"description": {
    	"type": "Text",
    	"value": "Restaurante de estilo sidrería ubicado en Alegria-Dulantzi. Además de su menú del día y carta, también ofrece menú de sidrería. El menú del día cuesta 9 euros. Los fines de semana la especialidad de la casa son las alubias con sacramentos. En lo que a bebidas se refiere, hay una amplia selección además de la sidra. Cabe destacar que se puede hacer txotx. La capacidad del establecimiento es de 50 personas pero la sidrería no dispone de aparcamiento.%5cn%5cnHORARIO: %5cn%5cnLunes a domingo: 9:00-17:00 y 19:00-23:00.",
    	"metadata": {

    	}
	},
	"location": {
    	"type": "geo:point",
    	"value": "42.8404625, -2.5123277",
    	"metadata": {

    	}
	},
	"name": {
    	"type": "Text",
    	"value": "Elizalde",
    	"metadata": {

    	}
	},
	"occupancyLevels": {
    	"type": "PropertyValue",
    	"value": 0,
    	"metadata": {
        	"timestamp": {
            	"type": "DateTime",
            	"value": "2016-09-29T14:19:39.267Z"
        	}
    	}
	},
	"priceRange": {
    	"type": "Number",
    	"value": 0,
    	"metadata": {

    	}
	},
	"telephone": {
    	"type": "Text",
    	"value": "945 400 868",
    	"metadata": {

    	}
	},
	"temperature:kitchen": {
    	"type": "Number",
    	"value": " ",
    	"metadata": {

    	}
	}
}
```

As we can see, a new attribute `temperature:kitchen` has been added to the restaurant entity.  Please note that the value is empty, as we have not sent any measurement yet.

## Update sensors

Once we have our sensors registered, we can begin sending measurements.  We can do this with the `tourguide` CLI by running:

```
./tour-guide sensors update
```
This will get the current value of the sensors, do a small variation and send a new measurement for each of the available sensors.  If we want to do this ourselves, we can send new measurements using the HTTP Ultralight 2.0 protocol.  See [Send data](#send-data) below for a detailed description on how to do this.

## Send data

If we want to update a single sensor, we can use the `send-data` command of `tourguide` CLI to do that:

```
$ ./tour-guide sensors send-data --help
Usage: tour-guide sensors send-data [-h | --help] [-i <sensorId> | --sensor-id <sensorId>]
                                	[ -d <ul20-string> | --data <ul20-string> ]

Send a single measurement for a specific sensor using a Ultralight 2.0 string.

Command options:

  -h  --help             	Show this help.

Required parameters:

  -i  --sensor-id  <sensorId> 	The sensor Id to modify.  The Id format is '<restaurantId>-<room>-<type>', with
                              	<restaurantId> being the Id of the restaurant where the sensor is located,
                              	<room> the room of the restaurant: kitchen, diner,
                              	<type> the type of the sensor: temperature, relativeHumidity.
  -d  --data <ul20-string>    	The string to send with the new measurement.  Examples of this are:
                              	't|20' for temperature (20 C),
                              	'h|0.4' for relativeHumidity (40%).

```

Here, we must specify the sensor Id and the new measurement using a Ultralight 2.0 string. Continuing with our example, imagine we want to send a temperature of 25 degrees for the sensor we created before.  The sensor Id was `0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature`, and the data string we must send is `t|25`:

```
./tour-guide sensors send-data -i 0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature -d 't|25'
```
If the request is successful, there will be no output.  If instead of using the `tourguide` CLI we want to send the measurement ourselves, we can do so by sending a POST request to `http://localhost:7896/iot/d` and add the following parámeters:

- k=${api_key}
- i=${sensor_id}

Here, the `${api_key}` value is the one we defined when registering the service.  In our example it's `tourguide-devices`.  The `${sensor_id}` is the Id of the sensor we want to update.  In our example it's `0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature`.

We also need to add the `Content-Type: text/plain` HTTP header to our request and send the Ultralight 2.0 data string as our payload:
```
curl -v -X POST -H 'content-type: text/plain' 'http://localhost:7896/iot/d?k=tourguide-devices&i=0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature' -d 't|25'
```

There will be no output if the request is successful. We can then check the restaurant to see if the value has been updated:
```
$ curl -s -H 'Fiware-Service: tourguide' http://localhost:1026/v2/entities/0115206c51f60b48b77e4c937835795c33bb953f | json_reformat
{
	"id": "0115206c51f60b48b77e4c937835795c33bb953f",
	"type": "Restaurant",
	"address": {
    	"type": "PostalAddress",
    	"value": {
        	"streetAddress": "Cuesta de las Cabras Aldapa 2",
        	"addressRegion": "Araba",
        	"addressLocality": "Alegría-Dulantzi",
        	"postalCode": "01240"
    	},
    	"metadata": {

    	}
	},
	"aggregateRating": {
    	"type": "AggregateRating",
    	"value": {
        	"ratingValue": 3,
        	"reviewCount": 64
    	},
    	"metadata": {

    	}
	},
	"capacity": {
    	"type": "PropertyValue",
    	"value": 200,
    	"metadata": {

    	}
	},
	"department": {
    	"type": "Text",
    	"value": "Franchise1",
    	"metadata": {

    	}
	},
	"description": {
    	"type": "Text",
    	"value": "Restaurante de estilo sidrería ubicado en Alegria-Dulantzi. Además de su menú del día y carta, también ofrece menú de sidrería. El menú del día cuesta 9 euros. Los fines de semana la especialidad de la casa son las alubias con sacramentos. En lo que a bebidas se refiere, hay una amplia selección además de la sidra. Cabe destacar que se puede hacer txotx. La capacidad del establecimiento es de 50 personas pero la sidrería no dispone de aparcamiento.%5cn%5cnHORARIO: %5cn%5cnLunes a domingo: 9:00-17:00 y 19:00-23:00.",
    	"metadata": {

    	}
	},
	"location": {
    	"type": "geo:point",
    	"value": "42.8404625, -2.5123277",
    	"metadata": {

    	}
	},
	"name": {
    	"type": "Text",
    	"value": "Elizalde",
    	"metadata": {

    	}
	},
	"occupancyLevels": {
    	"type": "PropertyValue",
    	"value": 0,
    	"metadata": {
        	"timestamp": {
            	"type": "DateTime",
            	"value": "2016-09-29T14:19:39.267Z"
        	}
    	}
	},
	"priceRange": {
    	"type": "Number",
    	"value": 0,
    	"metadata": {

    	}
	},
	"telephone": {
    	"type": "Text",
    	"value": "945 400 868",
    	"metadata": {

    	}
	},
	"temperature:kitchen": {
    	"type": "Number",
    	"value": "25",
    	"metadata": {

    	}
	}
}
```
As we can see, the value of the `temperature:kitchen` attribute has been updated to `25`.

## Simulate data

Finally, we can simulate a sensor behavior, by sending measurements of a sensor periodically by using the `simulate-data` command of `tourguide` CLI:

```
$ ./tour-guide sensors simulate-data --help
Usage: tour-guide sensors simulate-data [-h | --help] [-i <sensorId> | --sensor-id <sensorId>]
                                    	[ -t <type> | --type <type> ] [ -d <n> | --delay <n> ]

Simulate a sensor periodically sending data.

Command options:

  -h  --help             	Show this help.

Required parameters:

  -i  --sensor-id  <sensorId> 	The sensor Id to modify.  The Id format is '<restaurantId>-<room>-<type>', with
                              	<restaurantId> being the Id of the restaurant where the sensor is located,
                              	<room> the room of the restaurant: kitchen, diner,
                              	<type> the type of the sensor: temperature, relativeHumidity.
  -t  --type <type>           	Type of the sensor.  This must be the same type specified in the sensor Id.
                              	Valid values: temperature, relativeHumidity.
  -d  --delay <n>             	Delay in seconds between sensor readings.
```

This command will get the current value of the specified sensor, add or subtract a small value (between 0 and 5) and update the sensor, repeating the process at the specified intervals.  Following our example, we can do:
```
$ ./tour-guide sensors simulate-data -i 0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature -t 'temperature' -d 30
```
This will send a new `temperature` measurement of the `0115206c51f60b48b77e4c937835795c33bb953f-kitchen-temperature` sensor every `30` seconds.  To stop sending measurements, interrupt the command with `Control + C`.  This command uses the same method described in the previous section for sending the new measurements.  We can check the current value stored on the restaurant entity as before or use the following request to get just the value of the attribute:
```
curl -s -H 'Fiware-Service: tourguide' http://localhos835795c33bb953f/attrs/temperature:kitchen | json_reformat
{
	"type": "Number",
	"value": "25",
	"metadata": {

	}
}
```

<hr class="processing" style="display:none"/>
<h2>How to use other Generic Enablers from your Dashboard</h2>

WireCloud offers support for a number of GEs including the Context Broker and
the Object Storage GE. What follows is an example of how a developer can access
[Orion Context Broker](https://github.com/telefonicaid/fiware-orion) (reference
implementation of the FIWARE's Pub/Sub Context Broker GE) instances from widgets
and operators running on WireCloud. For more detailed information on how to use
this and other FIWARE GEs from WireCloud, please refer to WireCloud tutorials.

First of all, widgets and operators wishing to use the JavaScript bindings
provided by WireCloud for accessing the
[FIWARE NGSI Open RESTful API](<https://forge.fiware.org/plugins/mediawiki/wiki/fiware/index.php/FI-WARE_NGSI_Open_RESTful_API_Specification_(PRELIMINARY)>)
in order to seamlessly interoperate with the Orion Context Broker must add the
NGSI feature as a requirement into their description files (`config.xml` files).

The following is an example of a widget description using the XML flavour of the
WDL:

```xml
    <?xml version='1.0' encoding='UTF-8'?>
    <widget xmlns="http://wirecloud.conwet.fi.upm.es/ns/macdescription/1" vendor="CoNWeT" name="observation-reporter" version="1.0">
      <details>
        <title>Observation Reporter</title>
        <authors>aarranz</authors>
        <email>aarranz@conwet.com</email>
        <image>images/catalogue.png</image>
        <smartphoneimage>images/smartphone.png</smartphoneimage>
        <description>Creates a new observation</description>
        <doc>http://www.envirofi.eu/</doc>
      </details>
      <requirements>
        <feature name="NGSI"/>
      </requirements>
      <wiring/>
      <contents src="index.html" contenttype="text/html" charset="utf-8" useplatformstyle="true"/>
      <rendering height="20" width="5"/>
    </widget>
```

The RDF/xml flavour of the same widget description is:

```xml
    <?xml version='1.0' encoding='UTF-8'?>
    <rdf:RDF
      xmlns:foaf="http://xmlns.com/foaf/0.1/"
      xmlns:wire="http://wirecloud.conwet.fi.upm.es/ns/widget#"
      xmlns:rdfs="http://www.w3.org/2000/01/rdf-schema#"
      xmlns:usdl="http://www.linked-usdl.org/ns/usdl-core#"
      xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
      xmlns:ns1="http://purl.org/goodrelations/v1#"
      xmlns:dcterms="http://purl.org/dc/terms/"
      xmlns:vcard="http://www.w3.org/2006/vcard/ns#"
    >
      <wire:Widget rdf:about="http://wirecloud.conwet.fi.upm.es/ns/widget#CoNWeT/observation-reporter/1.0">
        <vcard:addr>
          <vcard:Work rdf:nodeID="Nb17ce611aa2645e488515f86eb855e53">
            <vcard:email>aarranz@conwet.com</vcard:email>
          </vcard:Work>
        </vcard:addr>
        <usdl:utilizedResource>
          <usdl:Resource rdf:about="index.html">
            <wire:codeCacheable>True</wire:codeCacheable>
          </usdl:Resource>
        </usdl:utilizedResource>
        <wire:hasPlatformWiring>
          <wire:PlatformWiring rdf:nodeID="Neecb97db81ed40859b8c04e935a9a9cc"/>
        </wire:hasPlatformWiring>
        <wire:displayName>Observation Reporter</wire:displayName>
        <wire:hasiPhoneImageUri rdf:resource="images/smartphone.png"/>
        <usdl:versionInfo>1.0</usdl:versionInfo>
        <usdl:hasProvider>
          <ns1:BusinessEntity rdf:nodeID="N9a6bf56577c741ac806997a80281afff">
            <foaf:name>CoNWeT</foaf:name>
          </ns1:BusinessEntity>
        </usdl:hasProvider>
        <wire:hasImageUri rdf:resource="images/catalogue.png"/>
        <wire:hasPlatformRendering>
          <wire:PlatformRendering rdf:nodeID="N713e5ea11dce4750a592c754c748def7">
            <wire:renderingHeight>20</wire:renderingHeight>
            <wire:renderingWidth>5</wire:renderingWidth>
          </wire:PlatformRendering>
        </wire:hasPlatformRendering>
        <wire:hasRequirement>
          <wire:Feature rdf:nodeID="N3cb336bd9b6243ecbf345c80442498f9">
            <rdfs:label>NGSI</rdfs:label>
          </wire:Feature>
        </wire:hasRequirement>
        <dcterms:title>observation-reporter</dcterms:title>
        <dcterms:description>Creates a new observation</dcterms:description>
        <dcterms:creator>
          <foaf:Person rdf:nodeID="Ndb72cb5a7f3844b29b72f304baaa14a7">
            <foaf:name>aarranz</foaf:name>
          </foaf:Person>
        </dcterms:creator>
      </wire:Widget>
    </rdf:RDF>
```

One of the most important operations provided by the context broker is the
support for subscriptions. By using subscriptions our dashboard can obtain right
time notifications about the status of the entities of interest. Subscriptions
are very similar to queries, the main difference between queries and
subscriptions is that queries are synchronous operations, whilst subscriptions
are asynchronous. Moreover, the Orion Context Broker will send a first
notification containing the data that would be returned for the equivalent query
operation. This way, you will know that there is no gap between the current
values and the notified changes.

Subscriptions are created through the `createSubscription` method. The following
example explains how to be notified about the changes of the position of the
vans we are managing:

```javascript
    var entityIdList = [
        {type: 'Van', id: '.*', isPattern: true}
    ];
    var attributeList = null;
    var duration = 'PT3H';
    var throttling = null;
    var notifyConditions = [{
        type: 'ONCHANGE',
        condValues: ['current_position']
    }];
    var options = {
        flat: true,
        onNotify: function (data) {
            // called when the context broker sends a new notification
        },
        onSuccess: function (data) {
            ngsi_subscriptionId = data.subscriptionId;
        }
    };
    var ngsi_connection = new NGSI.Connection(ngsi_server[, options]);
    ngsi_connection.createSubscription(entityIdList, attributeList, duration, throttling, notifyConditions, options);
```

In this example, the call to createSubscription will make the context broker
invoke the onNotify function each time the 'current_position' attribute of the
entities of type 'Van' is changed. You must take into account that the Orion
Context Broker evaluates patterns at runtime, so using patters you will be able
to receive notification about new entities provided that the notify conditions
are meet.

This subscription will be expiring after 3 hours, time from which the context
broker will stop sending notifications. Anyway, widgets/operators can renew
those subscriptions by using the `updateSubscription` method, even if they have
expired. Subscriptions can be cancelled using the `cancelSubscription` method
making the context broker release any info about the subscription. In any case,
WireCloud will cancel any subscription automatically when widgets/operators are
unloaded.

As an example, what follows is the value of the data parameter passed to the
onNotify callback when using the flat option:

```json
{
    "elements": {
        "van2": {
            "id": "van2",
            "type": "Van",
            "current_position": "43.47258, -3.8026643"
        },
        "van4": {
            "id": "van4",
            "type": "Van",
            "current_position": "43.471214, -3.7994885"
        }
    },
    "subscriptionId": "53708768286043030c116e2c",
    "originator": "localhost"
}
```

WireCloud tutorial on how to use Orion Context Broker from widgets and operators
further explain how to create connections, make queries, deal with pagination,
create entities and update their attributes, use geolocation capabilities, etc.
It also includes a complete example. Please also refer to the NGSI javascript
API documentation for a more detailed description of the API calls used along
the tutorial.

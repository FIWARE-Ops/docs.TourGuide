Connecting “objects” or “things” involves the need to overcome a set of
problems arising in the different layers of the communication model.
Using its data or acting upon them requires interaction with a
heterogeneous environment of devices running different protocols (due to
the lack of globally accepted standards), dispersed and accessible
through multiple wireless technologies.  

Devices have a lot of particularities so it is not feasible to provide a
solution where one size fits all. They are resource constrained and
can’t use full standard protocol stacks: they cannot transmit
information too frequently due to battery drainage, they are not always
reachable since they are connected through heterogeneous wireless
networks, their communication protocols are too specific and lack
integrated approach, and they use different data encoding languages, so
it is tricky to find a global deployment.  

There are three IoT typical use-case scenarios (combination of IoT GEs)
described in the [FIWARE IoT
architecture](http://forge.fiware.org/plugins/mediawiki/wiki/fiware/index.php/Internet_of_Things_%28IoT%29_Services_Enablement_Architecture).
The simplest and more tested one is called “Common Simple Scenario” and
it is depicted in the following picture: 

[![3](images/3.png)](images/3.png)

**FIWARE IoT Device Management GE architecture**

This simple scenario has got two basics
implementations:

-   **Orion ContextBroker:** That remains as the main front-end for
    developers. Developers access IoT data as attributes of entities
    representing devices and Developers may also send commands to
    devices by updating command-related attributes, providing they have
    access rights for that operation.
    You can see the lastest version [here](https://github.com/Fiware/context.Orion/tree/release/1.7.0)
-   **IDAS Backend Device Manager:** This component stays at the
    southbound of the Orion ContextBroker and it is used by IoT
    integrators to connect devices in this scenario.

IDAS supports several IoT protocols with a modular architecture where
modules are called “IoT Agents”. Therefore, integrators need to
determine first which protocol they will be using to connect devices and
select the right IoT Agent.   
 At present, the following IoT Agents and supported IoT protocols are:

-   **IDAS Ultralight2.0 & MQTT:** Supports Ultralight2.0/HTTP and/or
    MQTT (depends on the compilation options). Github repository is
    [here](https://github.com/Fiware/iot.IoTagent-UL).
    FIWARE Lab public instance details are
    [here](http://catalogue.fiware.org/enablers/backend-device-management-idas/instances). 
-   **IDAS LWM2M:** Supports OMA LightweightM2M over IETF CoAP (IPv4 and
    IPv6 supported). Github repository is
    [here](https://github.com/Fiware/iot.IoTagent-LWM2M).
    FIWARE Lab public instance details are
    [here](http://catalogue.fiware.org/enablers/backend-device-management-idas/instances).
-   **IDAS JSON:** This agent is designed to be a bridge between an MQTT+JSON based protocol and the FIWARE NGSI standard used in           FIWARE. Github repository is [here](https://github.com/Fiware/iot.IoTagent-JSON)
-   **IDAS Node Lib:** This is a Node.js module to enable IoT Agent developers to build custom agents for their devices that can easily     connect to NGSI Context Brokers (such as Orion ).Github repository is [here](https://github.com/Fiware/iot.IoTagent-node-lib)

For simplicity, in this article Ultralight2.0/HTTP is explained. To find out more information plese visit Github site.  
 If you are interested to connect your application to the Internet of
Things, keep on reading:

-   [How to read measures captured from IoT
    devices](/connection-to-the-internet-of-things/how-to-read-measures-captured-from-iot-devices/) 
-   [How to act upon IoT
    devices](/connection-to-the-internet-of-things/how-to-act-upon-iot-devices/)

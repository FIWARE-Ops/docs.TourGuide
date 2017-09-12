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

This simple scenario considers only two Generic Enablers
implementations:

-   **Orion ContextBroker:** That remains as the main front-end for
    developers. Developers access IoT data as attributes of entities
    representing devices and Developers may also send commands to
    devices by updating command-related attributes, providing they have
    access rights for that operation.
-   **IotAgents[here](https://github.com/Fiware?utf8=%E2%9C%93&q=IoTAgent):** Acting like translators 
    between devices specific platforms and Orion.
    

For simplicity, in this article Ultralight2.0/HTTP is explained. Please
refer to the FIWARE Catalogue to find out more information on the other
options.  
 If you are interested to connect your application to the Internet of
Things, keep on reading:

-   [How to read measures captured from IoT
    devices](/connection-to-the-internet-of-things/how-to-read-measures-captured-from-iot-devices/) 
-   [How to act upon IoT
    devices](/connection-to-the-internet-of-things/how-to-act-upon-iot-devices/)


If you want to start experimenting and doing hands-on work, have a look at:

- [IDAS GEri - IoT Agent](https://github.com/Fiware/iot.IoTAgent-Cplusplus)

As we did with the input endpoint, we need to declare the new output
endpoint in the weather widget's template. This is the final result of
the Wiring section after adding it:

    <wiring>
        <inputendpoint
            name="coord"
            type="text"
            label="Show forecast by coord"
            description="Shows the weather forecast for a given location (a latitude longitude coordinate)."
            friendcode="location"
        />
        <outputendpoint
            name="location_coord"
            type="text"
            label="Forecast location"
            description="This event is launched when the user clicks on the location name of current forecast."
            friendcode="location"
     />
    </wiring>

This is how to declare the output endpoint when using RDF (turtle):

    wire:hasPlatformWiring [ a <http:/​/wirecloud.conwet.fi.upm.es/ns/widget#PlatformWiring>;
            wire:hasInputEndpoint [ a <http:/​/wirecloud.conwet.fi.upm.es/ns/widget#InputEndpoint>;
                    rdfs:label "Show forecast by coord";
                    dcterms:description "Shows the weather forecast for a given location (a latitude longitude coordinate).";
                    dcterms:title "coord";
                    wire:friendcode "location";
                    wire:type "text" ] ];
            wire:hasOutputEndpoint [ a <http:/​/wirecloud.conwet.fi.upm.es/ns/widget#OutputEndpoint>;
                    rdfs:label "Forecast location";
                    dcterms:description "This event is launched when the user clicks on the location name of current forecast.";
                    dcterms:title "location_coord";
                    wire:friendcode "location";
                    wire:type "text" ];

After adding the output endpoint to the widget description, we can send
data through it using the MashupPlatform.wiring.pushEvent method. The
following code adds an event listener to the location title that sends
the location of the current forecast.

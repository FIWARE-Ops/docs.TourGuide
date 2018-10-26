Input endpoints must be declared into the widget template before it can be used
by the JavaScript code of the widget. To do so, open `config.xml` and add an
`<inputendpoint>` element into the `<wiring>` section. The final result should
look like:

    <wiring>
        <inputendpoint
            name="coord"
            type="text"
            label="Show forecast by coord"
            description="Shows the weather forecast for a given location (a latitude longitude coordinate)."
            friendcode="location"
        />
    </wiring>

This is how to declare the input endpoint when using RDF (turtle):

    wire:hasPlatformWiring [ a ;
            wire:hasInputEndpoint [ a ;
                    rdfs:label "Show forecast by coord";
                    dcterms:description "Shows the weather forecast for a given location (a latitude longitude coordinate).";
                    dcterms:title "coord";
                    wire:friendcode "location";
                    wire:type "text" ] ];

Once declared the input endpoint in the widget descriptor, you can register a
callback for this endpoint making use of the
`MashupPlatform.wiring.registerCallback` method. In addition to registering the
input endpoint, we need to process event data before using it and to notify the
user that the forecast data for the given location is being requested. This can
be accomplished by using the following code:

    var searchByCoordListener = function searchByCoordListener(event_data) {
        var tmp, coord;
        tmp = event_data.split(',');
        coord = {
            lat: tmp[1],
            lon: tmp[0]
        };
        startLoadingAnimation();
        getForecastByCoord(coord, function (forecast_data) {
            updateWeatherForecast(forecast_data);
            stopLoadingAnimation();
        }, function () {
            clearWeatherForecast();
            stopLoadingAnimation();
        });
    };

    MashupPlatform.wiring.registerCallback("coord", searchByCoordListener);

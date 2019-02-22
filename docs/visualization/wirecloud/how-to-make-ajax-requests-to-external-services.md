<hr class="processing" style="display:none"/>
<h2>How to make AJAX requests to external services</h2>

We will use the Weather Underground service to show how to make Ajax requests to
third party services. This service provides a rest API (documented at
http://www.wunderground.com/weather/api/d/docs), but it is impossible to access
this API using normal Ajax request (using XMLHttpRequest) due browsers applying
the “Same Origin Policy” to JavaScript code. Fortunately, WireCloud provides the
MashupPlatform.http.makeRequest method for dealing with this problem. A possible
way to access to this API is by using the following code:

```javascript
var getForecastByCoord = function getForecastByCoord(
    coord,
    onSuccess,
    onError
) {
    var url;
    if (typeof onSuccess !== "function" || typeof onError !== "function") {
        throw new TypeError();
    }
    url =
        "http://api.wunderground.com/api/" +
        API_KEY +
        "/conditions/forecast/q/";
    url += coord.lat + "," + coord.lon;
    url += ".json";
    MashupPlatform.http.makeRequest(url, {
        method: "GET",
        onSuccess: function(response) {
            var forecast_data;
            forecast_data = JSON.parse(response.responseText);
            if (forecast_data.error) {
                onError();
            } else {
                onSuccess(forecast_data);
            }
        },
        onError: function() {
            onError();
        }
    });
};
```

The getForecastByCoord function makes the appropriated request to Weather
Underground and passes the result to the onSuccess callback.

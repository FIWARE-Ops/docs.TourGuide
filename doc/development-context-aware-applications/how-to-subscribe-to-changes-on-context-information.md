Apart from getting information using queries in a synchronous way (as
illustrated in the “How to update and query context information” section
above), Context Consumers can also get context information in an
asynchronous way using notifications. In this scenario, the Context
Broker GE is “programmed” to send notifications upon given conditions
(specified in the subscription request).

In the case of NiceEating, the application backend could use a
subscription so each time a new review is cast by any user, the backend
gets notified (in order to recalculate restaurant average score and
publish it back in the Context Broker GE).

    POST <cb_host>:<cb_port>/v1/subscribeContext
        
    Headers:

    {
      'Content-Type':     'application/json',
      'Fiware-Service':   'tourguide'
    }

    Payload:

    {
      "entities": [
        {
          "type": "Review",
          "isPattern": "true",
          "id": ".*"
        }
      ],
      "attributes": [ "ratingValue"  ],
      "reference": "http://backend.niceeating.foo.com:1028/ratings",
      "duration": "P1M",
      "notifyConditions": [
        {
          "type": "ONCHANGE",
          "condValues": [
              "ratingValue"
          ]
        }
      ]
    }

Another case would be an application that subscribes to changes in
average ratings of a given restaurant. This may be useful for restaurant
owners in order to know how their restaurants score is evolving. In this example a subscription to the restaurant Elizalde which id is `0115206c51f60b48b77e4c937835795c33bb953f` is performed:
  
    POST <cb_host>:<cb_port>/v1/subscribeContext
        
    Headers:

    {
      'Content-Type':     'application/json',
      'Fiware-Service':   'tourguide'
    }

    Payload:

    {
      "entities": [
        {
          "type": "Restaurant",
          "isPattern": "false",
          "id": "0115206c51f60b48b77e4c937835795c33bb953f"
        }
      ],
      "attributes": [ "aggregateRating"  ],
      "reference": "http://myapp.foo.com:1028/restaurant_average_scorings",
      "duration": "P1M",
      "notifyConditions": [
        {
          "type": "ONCHANGE",
          "condValues": [
              "aggregateRating"
          ]
        }
      ]
    }

 

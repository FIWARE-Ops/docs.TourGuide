Apart from getting information using queries in a synchronous way (as
illustrated in the “How to update and query context information” section
above), Context Consumers can also get context information in an
asynchronous way using notifications. In this scenario, the Context
Broker GE is “programmed” to send notifications upon given conditions
(specified in the subscription request).

In the case of NiceEating, the application backend could use a
subscription so each time a new rating is cast by any user, the backend
gets notified (in order to recalculate restaurant average score and
publish it back in the Context Broker GE).

    POST <cb_host>:<cb_port>/v1/subscribeContext
    {
        "entities": [
            {
                "type": "Rating",
                "isPattern": "true",
                "id": ".*"
            }
        ],
        "attributes": [ "score"  ],
        "reference": "http://backend.niceeating.foo.com:1028/ratings",
        "duration": "P1M",
        "notifyConditions": [
            {
                "type": "ONCHANGE",
                "condValues": [
                    "score"
                ]
            }
        ]
    }

Another case would be an application that subscribes to changes in
average ratings of a given restaurant. This may be useful for restaurant
owners in other to know how their restaurants score is evolving.

    POST <cb_host>:<cb_port>/v1/subscribeContext
    {
        "entities": [
            {
                "type": "Restaurant",
                "isPattern": "true",
                "id": ".*"
            }
        ],
        "attributes": [ "average_scoring"  ],
        "reference": "http://myapp.foo.com:1028/restaurant_average_scorings",
        "duration": "P1M",
        "notifyConditions": [
            {
                "type": "ONCHANGE",
                "condValues": [
                    "average_scoring"
                ]
            }
        ]
    }

 

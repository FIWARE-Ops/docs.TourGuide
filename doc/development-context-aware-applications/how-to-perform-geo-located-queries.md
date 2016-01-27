One very powerful feature in Context Broker GE is the ability to perform
geo-located queries. You can query entities located inside (or outside)
a region defined by a circle or a polygon.  
 For example, to query for all the restaurants within 13 km of the
Madrid city center (identified by GPS coordinates 40.418889, -3.691944)
a Context Consumer application will use the following query:

    POST <cb_host>:<cb_port>/v1/queryContext
    {
        "entities": [
            {
                "type": "Restaurant",
                "isPattern": "true",
                "id": ".*"
            }
        ],
        "restriction": {
            "scopes": [
                {
                    "type": "FIWARE::Location",
                    "value": {
                        "circle": {
                            "centerLatitude": "40.418889",
                            "centerLongitude": "-3.691944",
                            "radius": "13000"
                        }
                    }
                }
            ]
        }
    }

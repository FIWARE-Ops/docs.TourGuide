One very powerful feature in Context Broker GE is the ability to perform
geo-located queries. You can query entities located inside (or outside) a region
defined by a circle or a polygon.  
 For example, to query for all the restaurants within 13 km of the Vitoria-Gasteiz
city center (identified by GPS coordinates 42.846718, -2.671635) a Context Consumer
application will use the following query:

    POST <cb_host>:<cb_port>/v1/queryContext

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
                "centerLatitude": 42.846718,
                "centerLongitude": -2.671635,
                "radius": "13000"
              }
            }
          }
        ]
      }
    }

To query for all restaurants inside a defined zone inside Vitoria-Gasteiz city a
Context Consumer application will use the following query:

    POST <cb_host>:<cb_port>/v1/queryContext

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
          "isPattern": "true",
          "id": ".*"
        }
      ],
      "restriction": {
        "scopes":[
          {
            "type": "FIWARE::Location",
            "value": {
              "polygon": {
                "vertices": [
                  {
                    "latitude": 42.847476,
                    "longitude": -2.763969
                  },
                  {
                    "latitude": 42.826006,
                    "longitude": -2.743151
                  },
                  {
                    "latitude": 42.826485,
                    "longitude":  -2.653740
                  },
                  {
                    "latitude": 42.867061,
                    "longitude": -2.630934
                  },
                  {
                    "latitude":  42.881801,
                    "longitude":  -2.640617
                  },
                  {
                    "latitude":   42.867767,
                    "longitude":   -2.726723
                  }
                ]
              }
            }
          }
        ]
      }
    }

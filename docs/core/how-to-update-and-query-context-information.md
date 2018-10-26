Processes running as part of your application architecture that update context
information using REST operations that the Context Broker GE exports, are said
to play a **Context Producer** role.  As an example, let’s consider an
application for rating restaurants (let’s call it NiceEating). The client part
of that application running on the smartphone of users would play the role of
Context Producer, enabling them to review restaurants.

On the other hand, processes running as part of your application architecture
that query context information using REST operations that the Context Broker GE
exports are said to play a **Context Consumer** role.  Continuing with our
NiceEating app, the mobile application running on the smartphone of users and
enabling them to query for the reviews of restaurants would play the role of
Context Consumer.  Note that a given part of your application may play both the
Context Producer and Context Consumer roles.  This would be the case of the
mobile client of the NiceEating app enabling end users to review and query about
reviews of restaurants.

Entities that would be relevant to the NiceEating application are of type
Restaurant, Client and Reviews. For example, when a given user reviews a
restaurant (e.g. in a scale from 0 to 5, “Client1234” rates “4” for the
“Elizalde” restaurant, which id is `0115206c51f60b48b77e4c937835795c33bb953f`,
and describes his/her experience with "Cheap and nice place to eat.") the
smartphone application plays the Context Producer role **creating** a Review
entity by issuing the following HTTP request:

    POST <cb_host>:<cb_port>/v1/contextEntities/type/Review/id/review-Elizalde-34

    Headers:

    {
      'Content-Type':     'application/json',
      'Fiware-Service':   'tourguide'
    }

    Payload:

    {
      "attributes":
      [
        {
          "name": "author",
          "type": "Person",
          "value": "Client1234"
        },
        {
          "name": "dateCreated",
          "type": "DateTime",
          "value": "2016-04-20T18:19:33.00Z"
        },
        {
          "name": "itemReviewed",
          "type": "Restaurant",
          "value": "0115206c51f60b48b77e4c937835795c33bb953f"
        },
        {
          "name": "reviewBody",
          "type": "Text",
          "value": "Cheap and nice place to eat."
        },
        {
          "name": "reviewRating",
          "type": "Rating",
          "value": 4
        }
      ]
    }

Each time a new Review entity is created, the average rating for the
corresponding restaurant is recalculated by the application backend which
(playing also the role of Context Producer) **updates** the Restaurant entity
accordingly. For this example the user has reviewed the Elizalde restaurant:

    PUT <cb_host>:<cb_port>/v1/contextEntities/type/Restaurant/id/0115206c51f60b48b77e4c937835795c33bb953f/attributes/aggregateRating

    Headers:

    {
      'Content-Type':       'application/json',
      'Fiware-Service':     'tourguide'
      'Fiware-ServicePath': '/Franchise1'
    }

    Payload:

    {
      "value" :{
        "ratingValue" : 4,
        "reviewCount": 1
      }
    }

Note that the `Fiware-ServicePath: /Franchise1` header is needed for update
operations because all restaurants are defined inside the scope of their
department. For further information about this option visit
<http://fiware-orion.readthedocs.io/en/develop/user/service_path/>.

Finally, the user can get the information of a given Restaurant using the
smartphone application. In that case the application works as Context Consumer,
**querying** the Restaurant entity. For example, to get the aggregateRating
attribute, the client application could query for it in the following way (with
extra headers `Fiware-Service: tourguide`):

    GET <cb_host>:<cb_port>/v1/contextEntities/type/Restaurant/id/0115206c51f60b48b77e4c937835795c33bb953f/attributes/aggregateRating

    {
      "attributes" : [
        {
          "name" : "aggregateRating",
          "type" : "StructuredValue",
          "value" : {
            "reviewCount" : 1,
            "ratingValue" : 4
          }
        }
      ],
      "statusCode" : {
        "code" : "200",
        "reasonPhrase" : "OK"
      }
    }

You can also obtain the values of all attributes of the "Elizalde" restaurant in
a single shot (with extra headers `Fiware-Service: tourguide`):

    GET <cb_host>:<cb_port>/v1/contextEntities/type/Restaurant/id/0115206c51f60b48b77e4c937835795c33bb953f

    {
      "contextElement" : {
        "type" : "Restaurant",
        "isPattern" : "false",
        "id" : "0115206c51f60b48b77e4c937835795c33bb953f",
        "attributes" : [
          {
            "name" : "address",
            "type" : "PostalAddress",
            "value" : {
              "streetAddress" : "Cuesta de las Cabras Aldapa 2",
              "addressRegion" : "Araba",
              "addressLocality" : "Alegría-Dulantzi",
              "postalCode" : "01240"
            }
          },
          {
            "name" : "aggregateRating",
            "type" : "StructuredValue",
            "value" : {
              "reviewCount" : 1,
              "ratingValue" : 4
            }
          },
          {
            "name" : "capacity",
            "type" : "PropertyValue",
            "value" : 50
          },
          {
            "name" : "department",
            "type" : "Text",
            "value" : "Franchise1"
          },
          {
            "name" : "description",
            "type" : "Text",
            "value" : "Restaurante de estilo sidrería ubicado en Alegria-Dulantzi. Además ..."
          },
          {
            "name" : "location",
            "type" : "geo:point",
            "value" : "42.8404625, -2.5123277"
          },
          {
            "name" : "name",
            "type" : "Text",
            "value" : "Elizalde"
          },
          {
            "name" : "occupancyLevels",
            "type" : "PropertyValue",
            "value" : 0,
            "metadatas" : [
              {
                "name" : "timestamp",
                "type" : "DateTime",
                "value" : "2016-04-20T18:15:15.434Z"
              }
            ]
          },
          {
            "name" : "priceRange",
            "type" : "Number",
            "value" : 0
          },
          {
            "name" : "telephone",
            "type" : "Text",
            "value" : "945 400 868"
          }
        ]
      },
      "statusCode" : {
        "code" : "200",
        "reasonPhrase" : "OK"
      }
    }

Alternatively, if you want to get the **attributes as a key map instead of as a
vector**, you can use the `attributesFormat` parameter, in the following way
(with extra headers `Fiware-Service: tourguide`):

    GET <cb_host>:<cb_port>/v1/contextEntities/type/Restaurant/id/0115206c51f60b48b77e4c937835795c33bb953f?attributesFormat=object

    {
      "contextElement" : {
        "type" : "Restaurant",
        "isPattern" : "false",
        "id" : "0115206c51f60b48b77e4c937835795c33bb953f",
        "attributes" : {
          "address" : {
            "type" : "PostalAddress",
            "value" : {
              "streetAddress" : "Cuesta de las Cabras Aldapa 2",
              "addressRegion" : "Araba",
              "addressLocality" : "Alegría-Dulantzi",
              "postalCode" : "01240"
            }
          },
          "aggregateRating" : {
            "type" : "StructuredValue",
            "value" : {
              "reviewCount" : 1,
              "ratingValue" : 4
            }
          },
          "capacity" : {
            "type" : "PropertyValue",
            "value" : 50
          },
          "department" : {
            "type" : "Text",
            "value" : "Franchise1"
          },
          "description" : {
            "type" : "Text",
            "value" : "Restaurante de estilo sidrería ubicado en Alegria-Dulantzi. Además ..."
          },
          "location" : {
            "type" : "geo:point",
            "value" : "42.8404625, -2.5123277"
          },
          "name" : {
            "type" : "Text",
            "value" : "Elizalde"
          },
          "occupancyLevels" : {
            "type" : "PropertyValue",
            "value" : 0,
            "metadatas" : [
              {
                "name" : "timestamp",
                "type" : "DateTime",
                "value" : "2016-04-20T18:15:15.434Z"
              }
            ]
          },
          "priceRange" : {
            "type" : "Number",
            "value" : 0
          },
          "telephone" : {
            "type" : "Text",
            "value" : "945 400 868"
          }
        }
      },
      "statusCode" : {
        "code" : "200",
        "reasonPhrase" : "OK"
      }
    }

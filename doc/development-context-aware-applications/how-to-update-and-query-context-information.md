Processes running as part of your application architecture that update
context information using REST operations that the Context Broker GE
exports, are said to play a **Context Producer** role.  As an example,
let’s consider an application for rating restaurants (let’s call it
NiceEating). The client part of that application running on the
smartphone of users would play the role of Context Producer, enabling
them to rate restaurants.

On the other hand, processes running as part of your application
architecture that query context information using REST operations that
the Context Broker GE exports are said to play a **Context Consumer**
role.  Continuing with our NiceEating app, the mobile application
running on the smartphone of users and enabling them to query for the
rating of restaurants would play the role of Context Consumer.  Note
that a given part of your application may play both the Context Producer
and Context Consumer roles.  This would be the case of the mobile client
of the NiceEating app enabling end users to rate, and query about rates
of, restaurants.

Entities that would be relevant to the NiceEating application are of
type Restaurant, Client and Rating. For example, when a given user
scores a restaurant (e.g. in a scale from 0 to 5, “Client1234” scores
“4” for the “LeBistro” restaurant) the smartphone application plays the
Context Producer role **creating**a Rating entity by issuing the
following HTTP request :

    POST <cb_host>:<cb_port>/v1/contextEntities/type/Rating/id/LeBistro::Client1234
    {
      "attributes" : [{
          "name" : "score",
          "type" : "integer",
          "value" : "4"
        }
      ]
    }

Each time a new Rating entity is created, the average rating for the
corresponding restaurant is recalculated by the application backend
which (playing also the role of Context Producer) **updates**the
Restaurant entity accordingly:

    PUT <cb_host>:<cb_port>/v1/contextEntities/type/Restaurant/id/LeBistro/attributes/average_scoring
    {
       "value" : "4.2"
    }

Finally, the user can get the information of a given Restaurant using
the smartphone application. In that case the application works as
Context Consumer, **querying**the Restaurant entity. For example, to get
the average\_scoring attribute, the client application could query for
it in the following way:

    GET <cb_host>:<cb_port>/v1/contextEntities/type/Restaurant/id/LeBistro/attributes/average_scoring
    //getting a JSON response such as the following one:
    {
      "attributes" : [
      {
        "name": "average_scoring",
        "type": "float",
        "value": "3.2"
      }
      ],
      "statusCode" : {
        "code" : "200",
        "reasonPhrase" : "OK"
      }
    } 

You can also obtain the values of all attributes of the "LeBistro"
restaurant in a single shot:

    GET <cb_host>:<cb_port>/v1/contextEntities/type/Restaurant/id/LeBistro

    //getting a JSON response such as the following one:

    {
      "contextElement": {
        "attributes": [
        {
          "name": "name",
          "type": "string",
          "value": "Le Bistro"
        },
        {
          "name": "average_scoring",
          "type": "float",
          "value": "4.2"
        },
        {
          "name": "location",
          "type": "location",
          "value": "40.419697, -3.691281",
          "metadatas": [
          {
            "name": "location",
            "type": "string",
            "value": "WGS84"
          }
        },
        {
          "name": "postal_address",
          "type": "string",
          "value": "Calle Alcala 57, 28028 Madrid, Spain"
        },
        {
          "name": "cousine_type",
          "type": "string",
          "value": "french"
        }
        ],
        "id": "LeBistro",
        "isPattern": "false",
        "type": "Restaurant"
      },
      "statusCode": {
        "code": "200",
        "reasonPhrase": "OK"
      }
    }

Alternatively, if you want to get the **attributes as a key map instead
of as a vector**, you can use the attributesFormat parameter, in the
following way

    GET <cb_host>:<cb_port>/v1/contextEntities/type/Restaurant/id/LeBistro?attributesFormat=object

    //getting a JSON response such as the following one:
    {
      "contextElement": {
        "attributes": {
          "name": {     
            "type": "string",
            "value": "Le Bistro"
          },
          "average_scoring": {
            "type": "float",
            "value": "4.2"
          },
          "location": {
            "type": "location",
            "value": "40.419697, -3.691281",
            "metadatas": [
            {
              "name": "location",
              "type": "string",
              "value": "WGS84"
            }
          },
          "postal_address": {
            "type": "string",
            "value": "Calle Alcala 57, 28028 Madrid, Spain"
          },
          "cousine_type": {
            "type": "string",
            "value": "french"
          }
        },
        "id": "LeBistro",
        "isPattern": "false",
        "type": "Restaurant"
      },
      "statusCode": {
        "code": "200",
        "reasonPhrase": "OK"
      }
    }

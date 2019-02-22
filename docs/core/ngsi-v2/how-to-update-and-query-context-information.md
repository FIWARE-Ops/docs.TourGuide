<hr class="core" style="display:none"/>
<h2>How to query and update context</h2>

Processes running as part of your application architecture that update context
information using REST operations that the Context Broker GE exports, are said
to play a **Context Producer** role.  As an example, let’s consider an
application for reviewing restaurants (let’s call it NiceEating). The client
part of that application running on the mobile phone of users would play the
role of Context Producer, enabling them to rate restaurants.

On the other hand, processes running as part of your application architecture
that query context information using REST operations that the Context Broker GE
exports are said to play a **Context Consumer** role.  Continuing with our
NiceEating app, the mobile application running on the mobile phone of users and
enabling them to query for the rating of restaurants would play the role of
Context Consumer.  Note that a given part of your application may play both the
Context Producer and Context Consumer roles.  This would be the case of the
mobile client of the NiceEating app enabling end users to review and query about
reviews of restaurants.

Entities that would be relevant to the NiceEating application are of type
Restaurant, Client and Review. In this guide, the restaurant Elizalde is used as
an example. In order to get its ID and department for performing the different
requests, we can use (with extra headers `Fiware-Service: tourguide`):

```
GET <cb_host>:<cb_port>/v2/entities?type=Restaurant&q=name==Elizalde&attrs=name,department&options=keyValues
```

```json
    [
      {
        "id": "0115206c51f60b48b77e4c937835795c33bb953f",
        "type": "Restaurant",
        "name": "Elizalde"
        "department": "Franchise1"
      }
    ]
```

When a given user reviews a restaurant (e.g. in a scale from 0 to 5,
“Client1234” scores “4” for the Elizalde restaurant and describes his/her
experience with "Cheap and nice place to eat.") the mobile phone application
plays the Context Producer role **creating** a Review entity by issuing the
following HTTP request :

```
POST <cb_host>:<cb_port>/v2/entities?options=keyValues
```

Headers:

```json
{
    "Content-Type": "application/json",
    "Fiware-Service": "tourguide"
}
```

Payload:

```json
{
    "type": "Review",
    "id": "review-Elizalde-34",
    "author": "Client1234",
    "itemReviewed": "0115206c51f60b48b77e4c937835795c33bb953f",
    "reviewBody": "Cheap and nice place to eat.",
    "ratingValue": 4
}
```

Each time a new Review entity is created, the average rating for the
corresponding restaurant is recalculated by the application backend which
(playing also the role of Context Producer) **updates** the Restaurant entity
accordingly:

```
PUT <cb_host>:<cb_port>/v2/entities/0115206c51f60b48b77e4c937835795c33bb953f/attrs/aggregateRating/value
```

Headers:

```json
    {
      'Content-Type':        'application/json',
      'Fiware-Service':      'tourguide'
      'Fiware-ServicePath':  '/Franchise1'
    }
```

Payload:

```json
{
    "reviewCount": 2,
    "ratingValue": 4
}
```

Note that the `Fiware-ServicePath: /Franchise1` header is needed for update
operations because all restaurants are defined inside the scope of their
department. For further information about this option visit
<http://fiware-orion.readthedocs.io/en/master/user/service_path/>.

Also, the user can get the information of a given Restaurant using the mobile
phone application. In that case, the application works as Context Consumer,
**querying** the Restaurant entity. For example, to get the aggregateRating
attribute, the client application could query for it in the following way (with
extra headers `Fiware-Service: tourguide`):

```
GET <cb_host>:<cb_port>/v2/entities/0115206c51f60b48b77e4c937835795c33bb953f/attrs/aggregateRating/value
```

```json
{
    "reviewCount": 2,
    "ratingValue": 4
}
```

You can also obtain the values of all attributes of the "Elizalde" restaurant in
a single shot (with extra headers `Fiware-Service: tourguide`):

```
GET <cb_host>:<cb_port>/v2/entities/0115206c51f60b48b77e4c937835795c33bb953f
```

```json
{
    "id": "0115206c51f60b48b77e4c937835795c33bb953f",
    "type": "Restaurant",
    "address": {
        "type": "PostalAddress",
        "value": {
            "streetAddress": "Cuesta de las Cabras Aldapa 2",
            "addressRegion": "Araba",
            "addressLocality": "Alegría-Dulantzi",
            "postalCode": "01240"
        },
        "metadata": {}
    },
    "aggregateRating": {
        "type": "StructuredValue",
        "value": {
            "reviewCount": 2,
            "ratingValue": 4
        },
        "metadata": {}
    },
    "capacity": {
        "type": "PropertyValue",
        "value": 80,
        "metadata": {}
    },
    "department": {
        "type": "Text",
        "value": "Franchise1",
        "metadata": {}
    },
    "description": {
        "type": "Text",
        "value": "Restaurante de estilo sidrería ubicado en Alegria-Dulantzi. Además ...",
        "metadata": {}
    },
    "location": {
        "type": "geo:point",
        "value": "42.8404625, -2.5123277",
        "metadata": {}
    },
    "name": {
        "type": "Text",
        "value": "Elizalde",
        "metadata": {}
    },
    "occupancyLevels": {
        "type": "PropertyValue",
        "value": 0,
        "metadata": {
            "timestamp": {
                "type": "DateTime",
                "value": "2016-04-20T18:15:15.434Z"
            }
        }
    },
    "priceRange": {
        "type": "Number",
        "value": 0,
        "metadata": {}
    },
    "telephone": {
        "type": "Text",
        "value": "945 400 868",
        "metadata": {}
    }
}
```

Alternatively, if you want to get only attribute values, you can use the
`keyValues` parameter, in the following way (with extra headers
`Fiware-Service: tourguide`):

```
GET <cb_host>:<cb_port>/v2/entities/0115206c51f60b48b77e4c937835795c33bb953f?options=keyValues
```

```json
{
    "id": "0115206c51f60b48b77e4c937835795c33bb953f",
    "type": "Restaurant",
    "address": {
        "streetAddress": "Cuesta de las Cabras Aldapa 2",
        "addressRegion": "Araba",
        "addressLocality": "Alegría-Dulantzi",
        "postalCode": "01240"
    },
    "aggregateRating": {
        "reviewCount": 2,
        "ratingValue": 4
    },
    "capacity": 80,
    "department": "Franchise1",
    "description": "Restaurante de estilo sidrería ubicado en Alegria-Dulantzi. Además ...",
    "location": "42.8404625, -2.5123277",
    "name": "Elizalde",
    "occupancyLevels": 0,
    "priceRange": 0,
    "telephone": "945 400 868"
}
```

Finally, the application can query entities by attribute content. For example,
to get all restaurants with capacity equal or greater than 50 the next request
can be used (with extra headers `Fiware-Service: tourguide`):

```
GET <cb_host>:<cb_port>/v2/entities?type=Restaurant&q=capacity>=50&options=keyValues
```

```json
[
    {
        "id": "0115206c51f60b48b77e4c937835795c33bb953f",
        "type": "Restaurant",
        "address": {
            "streetAddress": "Cuesta de las Cabras Aldapa 2",
            "addressRegion": "Araba",
            "addressLocality": "Alegría-Dulantzi",
            "postalCode": "01240"
        },
        "aggregateRating": {
            "reviewCount": 2,
            "ratingValue": 4
        },
        "capacity": 80,
        "department": "Franchise1",
        "description": "Restaurante de estilo sidrería ubicado en Alegria-Dulantzi. Además ...",
        "location": "42.8404625, -2.5123277",
        "name": "Elizalde",
        "occupancyLevels": 0,
        "priceRange": 0,
        "telephone": "945 400 868"
    },
    {
        "id": "34f036f9eef1f403c0ea66c55b8cdbace4b5f7a6",
        "type": "Restaurant",
        "address": {
            "streetAddress": "Avenida de Los Huetos 17",
            "addressRegion": "Álava",
            "addressLocality": "Vitoria-Gasteiz",
            "postalCode": "01010"
        },
        "aggregateRating": {
            "reviewCount": 1,
            "ratingValue": 3
        },
        "capacity": 50,
        "department": "Franchise4",
        "description": "El restaurante Araba cuenta con amplios salones para todo tipo de celebraciones, ...",
        "location": "42.8538811, -2.7006836",
        "name": "Araba",
        "occupancyLevels": 0,
        "priceRange": 42,
        "telephone": "945 222 669",
        "url": "http://www.restaurantearaba.com/"
    },
    {
        "id": "92e960de796a8cc48ed7c784119f7d5628e31a6f",
        "type": "Restaurant",
        "address": {
            "streetAddress": "Maskuribai Kalea",
            "addressRegion": "Araba",
            "addressLocality": "Amurrio",
            "postalCode": "01470"
        },
        "aggregateRating": {
            "reviewCount": 4,
            "ratingValue": 4
        },
        "capacity": 160,
        "department": "Franchise1",
        "description": "Bodega adscrita a la Denominación de Origen Arabako Txakolina y que elabora el 95% de la producción total ...",
        "location": "43.047415, -3.0007716",
        "name": "Arabako Txakolina, S.L.",
        "occupancyLevels": 0,
        "priceRange": 0,
        "telephone": "645 713 355",
        "url": "http://www.xarmant.net/"
    }
]
```

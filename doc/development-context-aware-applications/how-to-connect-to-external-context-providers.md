Context Broker GE also implements a mechanism so part of the context
information is not managed directly by the Context Broker, but by
external **Context Providers**. Thus, in this way, Context Broker GE
becomes the single access point  for all the context data , regardless
the actual location of the context source. This approach has several
advantages:

-   It’s easy for client applications to interact with just one API
    endpoint, instead of interacting with a different API endpoint for
    each source of context. Moreover, if the endpoint for a given source
    of context changes, the change is done at Context Broker GE in a way
    that is transparent for clients.
-   Scalability. When a single Context Broker GE is not enough to manage
    all the context information, several can be used. Each Context
    Broker GE (managing a subset of the context) is set as Context
    Provider in the Context Broker GE acting as single access point.
    This schema can be applied in a hierarchical way, which GE at
    different “level”, each level managing finer-grained context
    subsets.
-   Security. Clients may be not allowed to access directly to Context
    Providers for security reasons. In that case, the Context Broker GEs
    (which has the ability to authenticate and authorize requests) acts
    as security enforcement point.

Let’s illustrate with an example based on the NiceEating case. Let’s
consider an external restaurant booking system that is able to provide
in real time the occupancy level (as an absolute value). That system runs at
http://booking.restaurants.foo.com and plays the role of a Context
Provider.

First, the Context Provider is registered as provider of `occupancyLevels` for Elizalde restaurant which id is `0115206c51f60b48b77e4c937835795c33bb953f`:

    POST <cb_host>:<cb_port>/v1/registry/contextEntities/type/Restaurant/id/0115206c51f60b48b77e4c937835795c33bb953f/attributes/occupancyLevels
    {
      "duration" : "P1M",
      "providingApplication" : "http://booking.restaurants.foo.com"
    }

Thus, each time `occupancyLevels` is queried at Context
Broker GE (e.g. from the user smartphone application), that query is
forwarded to http://booking.restaurants.foo.com. From a Context Consumer
perspective, all the process is transparent, as if the information were
actually managed by Context Broker GE locally.

 

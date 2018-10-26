All the FIWARE Cloud capabilities outlined above can be invoked via OpenStack
APIs, with unique extensions currently available only in FIWARE. A typical
interaction would start by authenticating against the Identity Management GE,
and getting a token that can be used in consequent calls to the individual
services. For standard OpenStack APIs, one can use an existing client library -
one of the
[many maintained in the open source community](https://wiki.openstack.org/wiki/SDKs).

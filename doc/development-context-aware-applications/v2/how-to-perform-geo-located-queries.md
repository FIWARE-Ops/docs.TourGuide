One very powerful feature in Context Broker GE is the ability to perform
geo-located queries. You can query entities using the following spatial relationships: 

 * `georel=near`: proximity to a geometry. It supports the following modifiers:
    * `maxDistance`: express, in meters, the maximum distance at which matching entities must be located.
    * `minDistance`: express, in meters, the minimum distance at which matching entities must be located.
 * `georel=coveredBy`: covered by a defined geometry.
 * `georel=intersects`: intersected with a reference gometry.
 * `georel=equals`: equality to a geometry.
 * `georel=disjoint`: not intersected with the geometry.


 For example, to query for all the restaurants within 13 km of Vitoria-Gasteiz
 city center (identified by GPS coordinates 42.846718, -2.671635)
a Context Consumer application will use the following query:

    GET <cb_host>:<cb_port>/v2/entities?georel=near;maxDistance:13000&geometry=point&coords=42.846718,-2.671635


To query for all restaurants inside a defined zone inside Vitoria-Gasteiz city a Context Consumer application will use the following query: 

    GET <cb_host>:<cb_port>/v2/entities?georel=coveredBy&geometry=polygon&coords=42.847476,-2.763969;42.826006,-2.743151;42.826485,-2.653740;42.867061,-2.630934;42.881801,-2.640617;42.867767,-2.726723;42.847476,-2.763969


WebTundra GE is a Javascript library that uses three.js and
Synchronization GE to implement a Tundra client and WebGL rendering. It
takes care of asset loading, mapping three.js objects to Tundra EC:s,
handling networked changes to these in real time For a preview about
what you can do with three.js, look in http://threejs.org/examples/

For creating a WebTundra application, start from an existing three.js
WebGL application. Replication of objects and changes to their state are
handled by the network-aware Entity-Component system. For each
network-aware three.js 3D geometry object, an entity with Placeable and
Mesh components is created. Mesh component loads the 3D asset off the
web and creates a corresponding three.js mesh in the scene. Changing
Placeable attributes like position, scale, rotation are synchronized to
other clients via the server (and changed in the corresponding three.js
object).

For setting up, you need to have a server to connect to; either FI-LAB
or own machine, we also have a server running you can use right away.
Set up your local development environment and download WebTundra, or
develop a client on a FI-LAB vm - we have an installation recipe:  

https://forge.fi-ware.org/scmrepos/svn/testbed/trunk/cookbooks/GESoftware/webtundra/recipes/

 

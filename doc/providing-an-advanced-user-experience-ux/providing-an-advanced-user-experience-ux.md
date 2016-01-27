You may want to incorporate advanced features in your web-based user
interface, e.g., Augmented Reality or 3D visualization features. Then we
recommend you to take a look to the FIWARE GEs that have been defined in
the Advanced Web-based User Interface chapter. These components have
recently been incorporated in FIWARE so we are also working on their
integration with other FIWARE GEs. For example, you soon will be able to
fill information linked to POIs (Points Of Interest) visible in your
user interface using FIWARE Advanced Web-based User Interface GEs with
context information provided by the Context Broker GE.

Furthermore, your web based user interface can be a real-time
collaborative application utilizing the Synchronization GE. It features
automatic synchronization of your application or UI data between
multiple participants. This synchronization is already integrated with
the 3D-UI GE so that your 3D web application is a multiuser space out of
the box. This allows both the creation of collaborative applications
such as project work spaces and meeting tools, and realtime multiplayer
games.

The following diagram illustrates Advanced Web UI architecture and its
GEs.

[![Advanced user
experience](/uploads/2015/04/Advanced-user-experience-1024x549.png)](/uploads/2015/04/Advanced-user-experience.png)

The diagram shows how all the GEs from the WebUI chapter fit and can be
used together.

It is also possible and often suitable to use the GEs individually. For
example, if you make a single user traditional form based GUI
application for a mobile phone it can still use the POI GE for storing
and retrieving geopositioned information over standard HTTP. Or if you
wish to add 3D elements to your web page, without multiuser
functionality nor extra data backends, you can just use the 3D-UI GE by
itself.

You can use FILAB to test and develop on the advanced Web UI
technologies. It is also possible to install them on your own machines
and use web hosting services. For the Synchronization GE for multiuser
collaborative applications there is a commercial hosting provider
Meshmoon.com which is suitable for production use in business. Meshmoon
is also free to try: you can create an own collaborative 3D scene in a
minute!

Below we show discuss two different examples of using the Enablers
WebUI:

-   [3D-UI: Interactive 3D graphics and Augmented Reality in any Web
    browser](/providing-an-advanced-user-experience-ux/3d-ui-interactive-3d-graphics-and-augmented-reality-in-any-web-browser/)
-   [XML3D: Interactive 3D graphics and Augmented Reality via DOM
    extensions](/providing-an-advanced-user-experience-ux/xml3d-interactive-3d-graphics-and-augmented-reality-via-dom-extensions/xml3d-interactive-3d-graphics-and-augmented-reality-via-dom-extensions/)
    -   [​](/providing-an-advanced-user-experience-ux/xml3d-interactive-3d-graphics-and-augmented-reality-via-dom-extensions/xml3d-interactive-3d-graphics-and-augmented-reality-via-dom-extensions/)[How
        to set up a first XML3D
        scene](/providing-an-advanced-user-experience-ux/xml3d-interactive-3d-graphics-and-augmented-reality-via-dom-extensions/how-to-set-up-a-first-xml3d-scene/)
    -   [How to use Xflow for AR applications with XML3D and the
        Augmented Reality
        GE](/providing-an-advanced-user-experience-ux/xml3d-interactive-3d-graphics-and-augmented-reality-via-dom-extensions/how-to-use-xflow-for-ar-applications-with-xml3d-and-the-augmented-reality-ge/)
-   [3D graphics on the Web with
    Three.js](/providing-an-advanced-user-experience-ux/3d-graphics-on-the-web-with-three-js/)
-   [Multiuser 3D with WebTundra (Games, Virtual Worlds, Collaborative
    applications)](/providing-an-advanced-user-experience-ux/multiuser-3d-with-webtundra-games-virtual-worlds-collaborative-applications/)


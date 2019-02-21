<hr class="processing" style="display:none"/>
<h2>What is the Architecture of a Kurento-enabled Application</h2>

Kurento Media Server capabilities are exposed by the Kurento API to application
developers. This API is implemented by means of libraries called Kurento
Clients. Kurento offers two clients out of the box for Java and JavaScript. If
you have another favourite language, you can still use Kurento using directly
the Kurento Protocol. This protocol allows to control Kurento Media Server and
it is based on internet standards such as WebSocket and JSON-RPC. The picture
below shows how to use Kurento Clients in three scenarios:

-   Using the Kurento JavaScript Client directly in a compliant WebRTC browser
-   Using the Kurento Java Client in a Java EE Application Server
-   Using the Kurento JavaScript Client in a Node.js server

​[![Real time processin media
stream2](images/Real-time-processin-media-stream2.png)](images/Real-time-processin-media-stream2.png)

Kurento Client’s API is based on the concept of Media Element. A Media Element
holds a specific media capability. For example, the media element called
WebRtcEndpoint holds the capability of sending and receiving WebRTC media
streams, the media element called RecorderEndpoint has the capability of
recording into the file system any media streams it receives, the
FaceOverlayFilter detects faces on the exchanged video streams and adds a
specific overlaid image on top of them, etc. Kurento exposes a rich toolbox of
media elements as part of its APIs.

[![Real time processin media
stream3](images/Real-time-processin-media-stream3.png)](images/Real-time-processin-media-stream3.png)

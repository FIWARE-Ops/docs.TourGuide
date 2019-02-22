<hr class="processing" style="display:none"/>
<h2>How to develop a Kurento-enabled Application</h2>

From the application developer perspective, Media Elements are like Lego pieces:
you just need to take the elements needed for an application and connect them
following the desired topology. In Kurento jargon, a graph of connected media
elements is called a Media Pipeline. Hence, when creating a pipeline, developers
need to determine the capabilities they want to use (the media elements) and the
topology determining which media elements provide media to which other media
elements (the connectivity). The connectivity is controlled through the connect
primitive, exposed on all Kurento Client APIs. This primitive is always invoked
in the element acting as source and takes as argument the sink element following
this scheme:

```javascript
sourceMediaElement.connect(sinkMediaElement);
```

For example, if you want to create an application recording WebRTC streams into
the file system, you’ll need two media elements: WebRtcEndpoint and
RecorderEndpoint. When a client connects to the application, you will need to
instantiate these media elements making the stream received by the
WebRtcEndpoint (which is capable of receiving WebRTC streams) to be feed to the
RecorderEndpoint (which is capable of recording media streams into the file
system). Finally you will need to connect them so that the stream received by
the former is fed into the later:

```javascript
WebRtcEndpoint.connect(RecorderEndpoint);
```

To simplify the handling of WebRTC streams in the client-side, Kurento provides
an utility called WebRtcPeer. Nevertheless, the standard WebRTC API
(getUserMedia, RTCPeerConnection, and so on) can also be used to connect to
WebRtcEndpoints. For further information please visit our Kurento
[tutorials](https://www.kurento.org/docs/current/tutorials.html).

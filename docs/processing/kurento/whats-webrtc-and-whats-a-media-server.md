<hr class="processing" style="display:none"/>
<h2>What is WebRTC and what is a Media Server</h2>

WebRTC is an open source technology that enables web browsers with Real-Time
Communications (RTC) capabilities via JavaScript APIs. WebRTC is currently under
standardization at the IETF and W3C and has the support of the most important
companies in the area of internet and telecommunications. WebRTC has been
conceived as a peer-to-peer technology where browsers can directly communicate
without the mediation of any kind of infrastructure. This model is enough for
creating basic applications but features such as group communications, media
stream recording, media broadcasting or media transcoding are difficult to
implement on top of it. For this reason, many applications require using a media
server.

[![Real time processin media
stream1](images/Real-time-processin-media-stream1.png)](images/Real-time-processin-media-stream1.png)

Conceptually, a WebRTC media server is just a kind of “multimedia middleware”
(it is in the middle of the communicating peers) where media traffic pass
through when moving from source to destinations. Media servers are capable of
processing media streams and offering different types including groups
communications (distributing the media stream one peer generates among several
receivers, i.e. acting as Multi-Conference Unit, MCU), mixing (transforming
several incoming stream into one single composite stream), transcoding (adapting
codecs and formats between incompatible clients), recording (storing in a
persistent way the media exchanged among peers), etc.

Kurento, the Real-time Processing Stream Oriented GE, has been conceived as a
WebRTC capable media server. At the heart of the Kurento architecture there is a
media server called the **Kurento Media Server (KMS)**. Kurento Media Server is
based on pluggable media processing capabilities meaning that any of its
provided features is a pluggable module that can be activated or deactivated.
Moreover, developers can seamlessly create additional modules extending Kurento
Media Server with new functionalities which can be plugged dynamically.

Kurento Media Server provides, out of the box, group communications, mixing,
transcoding, recording and playing. In addition, it also provides advanced
modules for media processing including computer vision, augmented reality, alpha
blending and much more. Besides, Kurento Media Server is capable of receiving
video streams from most types of IP cameras supporting the RTSP or HTTP
protocols.

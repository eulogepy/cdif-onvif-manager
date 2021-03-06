Introduction
------------
CDIF's ONVIF module implementation

### Notes to ONVIF support in CDIF
The original ONVIF Profile S specification is written in WSDL and quite complex. It is possible to convert it into JSON format and present to clients, then map back to SOAP calls when clients need to access ONVIF based devices. However this approach maybe not make many sense because it adds extra efforts to client developers to get familiar with the whole ONVIF spec. Therefore, a simplified approach is taken such that we map [ONVIF library](https://github.com/agsh/onvif) APIs to CDIF's common device model. This gave us a much cleaner API interface, but for now only provides a few fundamental functionality because the underlying library implementation is still improving. In the future we may improve this over time while present clients a much simplified interface.

### Reverse proxy
This module implemented a reverse HTTP proxy server to help redirect incoming HTTP traffics to ONVIF device's configuration page while hiding its actual device URL. By doing this, imagine if CDIF runs on a home router, the camera device managed by CDIF would have no need to configure the device's DDNS or port forwarding etc to be accessible from external network. In addition, by doing this, CDIF may play a central security management role for all smart home and IoT devices to help better manage security risks which could be exposed by various different kinds of devices.

### Media streaming
By doing RTSP to MPEG1 transcoding through FFmpeg, the reverse HTTP proxy server in this module is capable of streaming MPEG1 based live video to client side through WebSocket transport. In this case, client side may integrate [jsmpeg library](https://github.com/phoboslab/jsmpeg) to play live HTML5 video in a canvas element. Under [examples/jsmpeg](https://github.com/out4b/cdif-onvif-manager/tree/master/examples/jsmpeg) there is an example of how to do this. Also by doing this, the video streams should be naturally encrypted in the production environment where the transport is based on TLS / https.

To enable this usage, the getStreamUri() action call through CDIF's RESTful interface needs to specify 'MPEG' and 'WebSocket' as input streamType and transport arguments.

See following links for more details: <br/>

[Common device interconnect framework](https://github.com/out4b/cdif)

[ONVIF node.js library](https://github.com/agsh/onvif)

# video-streaming
The Streaming Block takes an RTSP stream as an input and produces a WebRTC stream as an output. The input stream is selected automatically but can be overriden. If a container named "video-processing" is running on the device, it will use that block's RTSP output stream as an input. If no processing block is found, it will look for the [capture block](https://github.com/balenablocks/video-capture) and use that as the input. If neither are found, or you want to override this behavior, you can specify an RTSP input stream with the service variable `WEBRTC_RTSP_INPUT`. By default, the output WebRTC stream will be on port 80, but you can change that by specifying the service variable `WEBRTC_PORT`.

## Usage
To use this image, create a container in your `docker-compose.yml` file as shown below:
```
version: '2'

services:
  video-streaming:
    build: .
    network_mode: host
    labels:
      io.balena.features.supervisor-api: '1'
```

The streaming block utilizes [webrtc-streamer](https://github.com/mpromonet/webrtc-streamer) so all of its features are available to use as well. For instance, you can pass the options shown in [this list](https://github.com/mpromonet/webrtc-streamer#usage) using the `WEBRTC_OPTIONS` device variable. It must be used in conjunction with `WEBRTC_RTSP_INPUT` to create a full input for the webrtc-streamer. If you supply a value for `WEBRTC_OPTIONS` it will launch the following command, giving you full control over the streamer:
```
./webrtc-streamer WEBRTC_RTSP_INPUT WEBRTC_OPTIONS
```

So for instance if you want to specify an external TURN server, use:

`WEBRTC_RTSP_INPUT` = `-H 0.0.0.0:80` and `WEBRTC_OPTIONS` = `-sstun.l.google.com:19302` which is equivalent to:

```
./webrtc-streamer -H 0.0.0.0:80 -sstun.l.google.com:19302
```

## Companion blocks
The streaming block is designed to work seamlessly with our [capture block](https://github.com/balenablocks/video-capture) and a future processing block. In fact, version 2 of balenaSense is built using these two blocks along with just a few additional UI elements.

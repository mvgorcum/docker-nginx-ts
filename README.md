# Docker-nginx-rtmp
Docker image for an RTMP/HLS/MPEG-DASH server running on nginx

NGINX Version 1.18.0
nginx-rtmp-module Version 1.2.1

## Configurations
This image exposes port 1935 for RTMP Steams and has 2 default channels open "live" and "testing".

live (or your first stream name) is also accessable via MPEG-DASH and HLS on port 8080 under http://<your server ip>:8080/dash/live.mpd or http://<your server ip>:8080/hls/live.m3u8.

It also exposes 8080 so you can access http://<your server ip>:8080/stat to see the streaming statistics.

The configuration file is in /opt/nginx/conf/

At time of writing playback in Firefox 76 and chromium requires you to host [dash.js](https://github.com/Dash-Industry-Forum/dash.js).

## Running

To run the container and bind the port 1935 to the host machine; run the following:
```
docker run -p 1935:1935 -p 8080:8080 mvgorcum/nginx-rtmp:latest
```

### Multiple Streams:
You can enable multiple streams on the container by setting RTMP_STREAM_NAMES when launching, This is a comma seperated list of names, E.G.
```
docker run      \
    -p 1935:1935        \
    -p 8080:8080        \
    -e RTMP_STREAM_NAMES=live,teststream1,teststream2   \
    mvgorcum/nginx-rtmp
```

### Pushing streams
You can ush your main stream out to other RTMP servers, Currently this is limited to only the first stream in RTMP_STREAM_NAMES (default is live) by setting RTMP_PUSH_URLS when launching, This is a comma seperated list of URLS, EG:
```
docker run      \
    -p 1935:1935        \
    -p 8080:8080        \
    -e RTMP_PUSH_URLS=rtmp://live.youtube.com/myname/streamkey,rtmp://live.twitch.tv/app/streamkey
    mvgorcum/nginx-rtmp
```

## OBS Configuration
Under broadcast settigns, set the follwing parameters:
```
Streaming Service: Custom
Server: rtmp://<your server ip>/live
Play Path/Stream Key: mystream
```

## Watching the steam

In your favorite RTMP video player connect to the stream using the URL:
rtmp://<your server ip>/live/mystream
http://<your server ip>/dash/mystream.mpd
http://<your server ip>/hls/mystream.m3u8


## Tested players
 * VLC
 * Firefox (with hosted dash.js)
 * Chromium (with hosted dash.js)

# GDG Streaming Setup

## Description

This streaming setup is using Docker (and Docker Compose) to deploy easily all the needed services.

### How to deploy

Note : You will need to setup NodeCG before deploying containers.

````sh
$> docker-compose up -d
````

## Services

### RTMP Server (Nginx)

An RTMP server is available (using Nginx).

This server is only used to handle RTMP, since only port 1935 is exposed.

Its default configuration does not accept stream.
To be able to stream content, you will have to use `stream` script to generate an endpoint.

#### How to manage streams

````sh
$> docker exec -it $CONTAINER_NAME stream $ARGS
````

##### Arguments

- `--twitch $STREAMING_KEY` : Twitch Streaming key
- `--twitch-server $SERVER_URL` : Twitch Streaming server (Default : `rtmp://live-cdg.twitch.tv/app/`)
- `--twitch-test` : Enable Bandwidth test flag for Twitch stream
- `--youtube $STREAMING_KEY` : YouTube Streaming key
- `--youtube-server $SERVER_URL` : YouTube Streaming server (Default : `rtmp://a.rtmp.youtube.com/live2/`)
- `--facebook $STREAMING_KEY` : Facebook Streaming key
- `--facebook-server $SERVER_URL` : Facebook Streaming server (Default : `rtmp://live-api-s.facebook.com:80/rtmp/`)
- `-v, --verbose` : Verbose mode

##### Modes

- With arguments

This mode will generate a new streaming endpoint (Endpoint will be displayed when generated) and setup streaming redirection to specified platforms.
For more details, please check arguments list.

Example :

````sh
$> # To stream on Twitch
$> docker exec -it $CONTAINER_NAME stream --twitch $STREAMING_KEY
Streaming endpoint : XXXXXXXXXXXXXXXXXXXX

$> # To stream on YouTube
$> docker exec -it $CONTAINER_NAME stream --youtube $STREAMING_KEY
Streaming endpoint : XXXXXXXXXXXXXXXXXXXX

$> # To stream on Twitch & YouTube
$> docker exec -it $CONTAINER_NAME stream --twitch $STREAMING_KEY --youtube $STREAMING_KEY
Streaming endpoint : XXXXXXXXXXXXXXXXXXXX

$> # To stream on 2 Facebook accounts
$> docker exec -it $CONTAINER_NAME stream --facebook $STREAMING_KEY_1 --facebook $STREAMING_KEY_2
Streaming endpoint : XXXXXXXXXXXXXXXXXXXX

````

- Without arguments

This mode will clear RTMP server configuration by removing any streaming endpoint.

Example :

````sh
$> # To clear stream
$> docker exec -it $CONTAINER_NAME stream
````

### NodeCG

NodeCG is a Node.JS based server to manage visual items (bundles).

Its dashboard is available on port 9090.

#### How to setup

##### Authentication setup

You will need to generate a secret and put it as value for `sessionSecret` in `nodecg/config.json`.

To add users, you can set accounts in `allowedUsers` list. Users must have the following structure :
````json
{
    "username": "username",
    "password": "password"
}
````

#### Bundles

##### Status Message

This bundle handles status message (e.g. "Début de stream dans un instant").
It has 3 basic modes (Incoming, AFK, End) and a Clear mode, but you can also customize status message using input fields.

## TODO

- [ ] Chatroom integration
- [ ] Twitch notifications (Follow, subscription, bits, ...)
- [ ] YouTube notifications (Subscribe, like, ...)
- [ ] Facebook notifications (Subscribe, like, ...)
- [ ] ...

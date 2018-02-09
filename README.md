# rgb-iot

A project that will employ a small Fog(mini-cloud) network. The system consists of:
An MQTT broker, NodeJS server(rgb-agent), web client and Arduinos with LEDs

### Arduino Device
Currently I am using Adafruit feathers. 
These devices have at least 3 LEDs connected to them: red, green, blue. 
Each device is referred to as 'edge' and the designated number edge-1, edge-2, edge-3.

### MQTT broker
Currently runs on an Intel Edison, the software it is using is called Mosquitto.
This broker provides publication and subscription services for all edge devices

### Node server, rgb-agent
 * Provides a web page, that will function as a control panel and information display
 * Sends MQTT topics on behalf of a user to an MQTT broker, 
 * Enables user control of the red, green and blue lights on an Arduino device.

### The cloud that the fog communicates with
Eventually this fog will send messages up to the cloud. 
There are a number of solutions out there to achieve this is a WIP (work in progress)

## rgb-agent The API

A user of the system has access to a page with various user interface.
These various UI send messages to the Node servers' API. 

This API is as follows:

POST
    /application-name/device-name/property-name/sub-property-name/value [ 0 - 255 ]
POST
    /application-name/device-name/property-name/value [ 0 - 255 ]

Target a characteristic of a light on a device in the context of the application 'rgb'
    /rgb/edge-1/light/red/100

Target a characteristic of a fan on a particular device in the context of the application 'rgb'
    /rgb/edge-1/fan/on

API calls for controlling distinct LEDs
    /rgb/edge-1/light/red/VALUE [ 0 - 255 ]
    /rgb/edge-1/light/green/VALUE [ 0 - 255 ]
    /rgb/edge-1/light/blue/VALUE [ 0 - 255 ]

API paths for controlling the effects
    /rgb/edge-1/effect/glow/VALUE [ on off]
    /rgb/edge-1/effect/march/VALUE [ on off]

The API calls for the most part will result in the emitting of MQTT topics.
These calls will start and stop functions that will drive the LEDs "effects" or simply set their value.

### The MQTT topics and value ranges.

Once the Node server API is posted to, an MQTT call is made in the form:

'application-name:device-name:property-of-device:sub-property' -m value

Examples include:
    'rgb:edge-1:light:red' -m 0
    'rgb:edge-1:light:green' -m 100
    'rgb:edge-1:light:blue' -m 200


### The User interface and Client

Currently the UI consists of control for the distinct LEDs, red, green, blue 
and control to start and stop effects.

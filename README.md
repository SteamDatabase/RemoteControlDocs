# Steam Remote API Documentation

## Introduction
This is an overview of the RESTful API for the Steam Remote HTTP API, first documented by [SteamDB](http://steamdb.info/blog/31/) back in October 2013. They later released [another blogpost](http://steamdb.info/blog/37/) with more information and a link to a repository providing some more functionality than that which is in the _headsDown.html_ file provided with Steam as of December 19th.

Credit mostly goes to the SteamDB guys for this, they did all the leg-work. just extracted the information from their [SteamRemoteClient Tool](https://github.com/SteamDatabase/RemoteUI/) and turned it into a REST Documentation.

## Table of Contents
+ [Overview](#overview)
+ [Authentication](#authentication)
+ [Buttons](#buttons)
+ [Mouse](#mouse)
    - [Mouse Movement](#mouse-movement)
    - [Mouse Buttons](#mouse-buttons)
+ [Keyboard](#keyboard)
	- [Keys](#keys)
	- [Sequences](#sequences)
+ [Games](#games)
	- [List Games](#list-games)
	- [Run Games](#run-game)
+ [Spaces](#spaces)
	- [Current Space](#current-space)
	- [Change Space](#change-space)
+ [Client Libraries](#client-libraries)

## <a name="overview"></a>Overview

### Enabling Remote Control
Launch Steam with the `-enableremotecontrol` argument. This will launch Steam with the remote webserver enabled so you can access all the new API features. Alternatively those running SteamOS will find this is enabled by default.

### Formats
The base URL for all API requests is `https://<your_steam_client_ip>:8443/steam`

Data is sent as GET or POST payloads, and returned as [JSON](http://json.org).

### Errors
Supplying incorrect payload data or using an invalid API call will result in a HTTP `404` Error Code being returned.

### Success Response
Unless otherwise stated, it can be assumed that each API call simply returns a JSON response of the following on a successful call:

```json
{
    "success": true
}
```

## Authentication
The first request you make to API will (providing you're in Big Picture) prompt you to authorise the remote client by its device name and token. Every API request requires the passing of at least a device token as GET, POST or COOKIE data and will therefore not be included in the documentation beyond the below as it assumed you are already including these details appropriately for each request.

#### Parameters
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Required?</th>
            <th>Type</th>
            <th width=100%>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>device_name</code></td>
            <td>optional</td>
            <td>string</td>
            <td>Device Name as it will appear in the Steam Big Picture prompt during authorisation. "Unkown Device" used if not supplied.</td>
        </tr>
        <tr>
            <td><code>device_token</code></td>
            <td>required</td>
            <td>string</td>
            <td>Device Token used to authorise your client with Steam. This can be generated locally but must be the same for each request (or authorisation will be required again).</td>
        </tr>
    </tbody>
</table>

## Buttons
```
POST /steam/button/:button/
```
The Button API allows you to simulate the pressing of controller buttons within Steam Big Picture. The buttons are based on an Xbox 360 Gamepad, whether this will change in the future to better reflect the official Steam Controller is unknown.

#### Valid Buttons

The button identifier should be used in place of :button in the get request above.

<table>
    <thead>
        <tr>
            <th>Button</th>
            <th>Identifier</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Left</td>
            <td><code>left</code></td>
        </tr>
        <tr>
            <td>Right</td>
            <td><code>right</code></td>
        </tr>
        <tr>
            <td>Up</td>
            <td><code>up</code></td>
        </tr>
        <tr>
            <td>Down</td>
            <td><code>down</code></td>
        </tr>
        <tr>
            <td>A</td>
            <td><code>right</code></td>
        </tr>
        <tr>
            <td>B</td>
            <td><code>b</code></td>
        </tr>
        <tr>
            <td>X</td>
            <td><code>x</code></td>
        </tr>
        <tr>
            <td>Y</td>
            <td><code>y</code></td>
        </tr>
    </tbody>
</table>

## Mouse
The Mouse API can be used to control the cursor within Steam Big Picture and simulate Mouse Button presses.

### Mouse Movement
Moves the Big Picture cursor x/y distance relative to its current position.

#### Parameters
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Required?</th>
            <th>Type</th>
            <th width=100%>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>delta_x</code></td>
            <td>required</td>
            <td>int</td>
            <td>Distance to move the mouse on the x-axis</td>
        </tr>
        <tr>
            <td><code>delta_y</code></td>
            <td>required</td>
            <td>int</td>
            <td>Distance to move the mouse on the y-axis</td>
        </tr>
    </tbody>
</table>

### Mouse Buttons
```
POST /steam/mouse/click
```
Simulates a mouse click in Steam Big Picture.
#### Parameters
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Required?</th>
            <th>Type</th>
            <th width=100%>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>button</code></td>
            <td>required</td>
            <td>string</td>
            <td>The mouse button to simulate a press for, currently tested: <code>mouse_left</code></td>
        </tr>
    </tbody>
</table>

## Keyboard
The Keyboard API allows you to simulate key presses (or a sequence of characters being typed out) within Steam Big Picture.

### Keys
TODO.

### Sequences
```
POST /steam/keyboard/sequence/
```
The Keyboard API's sequence method accepts a string of characters as a POST field and outputs it as a string of characters in the current selected text input within Steam Big Picture.

#### Parameters
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Required?</th>
            <th>Type</th>
            <th width=100%>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>sequence</code></td>
            <td>required</td>
            <td>string</td>
            <td>The string of unicode characters to enter into the currently selected input field.</td>
        </tr>
    </tbody>
</table>
## Games
The Games API allows you to browse your game library remotely and even load up games within Steam Big Picture.
### List Games
```
GET /steam/games/
```
Returns a list of all available games in the currently logged in user's library. Each key represents a Steam App ID.

#### Sample Response
```json
{
    "success": true,
    "data": {
        "3483": {
            "name": "Peggle Extreme",
            "installed": 1,
            "update_running": 0,
            "update_paused": 0,
            "bytes_downloaded": 14690192,
            "bytes_needed": 14690192,
            "bytes_per_second": 0,
            "type": "game",
            "icon": "http://media.steampowered.com/steamcommunity/public/images/apps/3483/427a98a549c3813e13b4062300709000599817b0.jpg",
            "logo": "http://media.steampowered.com/steamcommunity/public/images/apps/3483/4b8f3d8f7f94cc5ca420a8586c8dd903edacce12.jpg",
            "current_disk_bytes": 23356380,
            "estimated_disk_bytes": 23356380,
            "minutes_played": {
                "forever": 47,
                "last_two_weeks": 46
            },
            "last_played_at": 1387758040
        }
    }
}
```
### Run Game
```
POST /steam/games/:appid/run
```
Runs the game in the logged in user's library corresponding to the supplied App ID (if it's installed).

## Spaces

#### Known Space Mappings
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Identifier</th>
            <th width=100%>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Library</td>
            <td>library</td>
            <td>Currently the default space and associated with the store, game library and community sections.</td>
        </tr>
        <tr>
            <td>Friends</td>
            <td>friends</td>
            <td>The space from where you can send and receive messages and interact with your friends list.</td>
        </tr>
        <tr>
            <td>Web Browser</td>
            <td>webbrowser</td>
            <td>The Steam Big Picture web browser.</td>
        </tr>
    </tbody>
</table>

### Current Space
```
GET /steam/space/
```
Gets the currently active space from Big Picture. Refer to the mappings table above for more information.

#### Sample Response
```json
{
    "success": true,
    "data": { 
        "name": "library"
    }
}
```
### Change Space
```
POST /steam/space/
```
Changes the currently active space in Big Picture.

#### Parameters
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Required?</th>
            <th>Type</th>
            <th width=100%>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>space</code></td>
            <td>required</td>
            <td>string</td>
            <td>The space identifier to navigate to. Refer to the mappings table above.</td>
        </tr>
    </tbody>
</table>

## Client Libraries
### Javascript
#### [SteamDB's SteamRemoteClient](https://github.com/SteamDatabase/RemoteUI/blob/master/js/RemoteClient.js)
Primarily made for use with the RemoteUI.html tool (included in the repository), but for non-local access can be easily modified. It's worth noting that the API does not provide an `Access-Control-Allow-Origin` header, let alone one with a wildcard value, so most browsers won't work with it from a different origin without disabling some security features. JSONP doesn't support POST requests, so that option is out the window too.

======

If you have a library, please do add it above under the necessary language heading (if the language isn't there, add it)!
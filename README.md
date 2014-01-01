# Steam Remote API Documentation

## Introduction
This is an overview of the RESTful API for the Steam Remote HTTP API, first documented by [SteamDB](http://steamdb.info/blog/31/) back in October 2013. They later released [another blogpost](http://steamdb.info/blog/37/) with more information and a link to a repository providing some more functionality than that which is in the _headsDown.html_ file provided with Steam as of December 19th.

## Table of Contents
+ [Overview](#overview)
+ [Authentication](#authentication)
+ [Buttons](#buttons)
+ [Keyboard](#keyboard)
	- [Keys](#keyboard-keys)
	- [Sequences](#keyboard-sequences)
+ [Games](#games)
	- [List Games](#games-list)
	- [Play Games](#games-play)
+ [Space](#space)
	- [Current Space](#space-current)
	- [Change Space](#space-change)
+ [Client Libraries](#client-libraries)

## <a name="overview"></a>Overview

### Formats
The base URL for all API requests is `https://<your_steam_client_ip>:8443/steam`

Data is sent as GET or POST payloads, and returned as [JSON](http://json.org).

### Errors
Supplying incorrect payload data or using an invalid API call will result in a HTTP `404` Error Code being returned.

## <a name="authentication"></a>Authentication
The first request you make to API will (providing you're in Big Picture) prompt you to authorise the remote client by its device name and token. Every API request requires the passing of at least a device token as GET, POST or COOKIE data and will therefore not be included in the documentation beyond the below as it assumed you are already including these details appropriately for each request.

### Parameters
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Required?</th>
            <th width="50">Type</th>
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
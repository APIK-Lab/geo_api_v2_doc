# Geo API v2 documentation

_Updated on March the 31th 2023_

This API allow users to retreive raw data transmitted by their geo capable
devices. This API supports following device types :

* APIK KIP ;
* APIK MARKER.

The complete Swagger doc is available at : 
[Geo API v2 Swagger doc](https://api.api-k.com/geo/v2/swagger/).

## General informations

### Authentication

Authentication is a basic one. To retreive your credentials, you'll have to
connect to Maker portal (soon released). Basic authentication is made of a
username and a password. username is unmutable. Password can be one of those :

* rw primary key : primary key to consume data with no restriction ;
* rw secondary key : secondary key to consume data with no restriction ;
* ro primary key : primary key to consume data in readonly mode ;
* ro secondary key : secondary key to consume data in readonly mode.

For each scope (rw or ro), primary and secondary can be used equaly. It allows
you to roll regulary keys with no service interruption.

### Rate limititation

It is worth noting that this api is rate limited. By default, every account is
limited at 1 request per second. If you need more, please [contact us](#contact-us).

### FIQL

Some requests allow user to complete the request with a FIQL query. FIQL stands
for "Feed Item Query Language", and it is a query language that is used to
filter and search for items in RSS and Atom feeds.

FIQL was designed to be a simple and lightweight language for expressing filter
conditions that can be applied to feed items. It allows users to search for
specific content within feeds based on different criteria, such as keywords,
dates, categories, and authors.

FIQL uses a syntax similar to the URL query string, with different query
parameters separated by ampersands (&). For example, a basic FIQL query for
finding all items that contain the word "technology" in the title could look
like this:

```bash
/devices?query=model=="K-IP"
```

FIQL also supports more complex queries with logical operators such as "AND",
"OR", and "NOT", as well as comparison operators such as "equals", "contains",
"less than", and "greater than".

Overall, FIQL provides a simple and flexible way for developers and users to
search and filter feed content, making it easier to find the information they
need.

If you plan to use FIQL to request our data, you can read the doc at
[IETF](https://datatracker.ietf.org/doc/html/draft-nottingham-atompub-fiql-00).

### OnPremise

This API will be available completly in the cloud. But, some resources could be
unavailable on premise. In this case, a disclaimer will be mentionned in the
resource description. Moreover, the resource won't be mentionned in the swagger
doc published on premise.

## Devices

The first endpoint is `devices`.

### /devices

By requesting `/devices`, you'll have the opportunity to request all information
of each device :

* `id` : this information is used to identify the device on the server side
(and all other resources) ;
* `eui` : this is the unique id which is physically written on the device ;
* `manufacturer` and `model` are obvious !

This is a KIP :

![](img/kip.png)

The `EUI` is just under the QR-Code at the back of the device :
`ecdb86ffff00037b`.

Please note that if you have to query your devices based upon a selection,
you'll be able to use a FIQL query. For example, to request every devices that
are KIP, the request will be : `/devices?query=model=="K-IP"`. Dont forget to
URL encode this request. Once done, it will be : 
`/devices?query=model%3D%3D%22K-IP%22`.

### /devices/\{id\}

This request is the same than the the previous except that you choose 
specifically to request one device using its `id`.

### /devices/\{id\}/last_data

This endpoint allows you to retreive last data for a specific device. Please
note that whereas this resource can be used, if you need to request the last
data of all you devices, we encourage you to use [`/last_data`](#/last_data).

### /devices/\{id\}/data

**This resource will not be available on premise !!!**

This resource allows user to query devices data on a period. Without any other
informations, this resource will return last dta. But, if you specify a FIQL
query, you'll be able to retreive more informations :

```bash
/devices/{id}/data?query=ts=gt="2023-03-29T07:56:00.0000000Z"
```

Please note that the resultset returned by this query will be limited to the 200
last datum. If you need more, you'll have to make the window sliding :

```bash
/devices/{id}/data?query=ts=gt="2023-03-29T07:56:00.0000000Z";ts=lt="2023-03-31T07:56:00.0000000Z"
/devices/{id}/data?query=ts=gt="2023-03-29T07:56:00.0000000Z";ts=lt="2023-03-30T07:56:00.0000000Z"
```

## Last data

This endpoint currently handle only one simple resource `/last_data`.

### /last_data

The resource allow to retreive in one call the last data of each device. We
strongly encourage you to use this endpoint as far as it'll scale very easily.
This endpoint will return you the data like this :

```json
[
  {
    "device_id": 1730,
    "rxts": "2023-03-31T05:51:26.921Z",
    "ts": "2023-03-31T05:51:26.921Z",
    "nwk": "lorawan",
    "status": {
      "mode": "activated",
      "mov": false
    },
    "position": {
      "ts": "2023-03-30T18:43:51Z",
      "lat": 48.08786,
      "lon": 3.505139
    }
  },
  {
    "device_id": 361,
    "rxts": "2023-03-31T05:51:26.323Z",
    "ts": "2023-03-31T05:51:26.323Z",
    "nwk": "lorawan",
    "status": {
      "mode": "activated",
      "mov": false
    },
    "position": {
      "ts": "2023-03-30T18:43:28.737Z",
      "lat": 45.888743,
      "lon": 5.200779
    }
  }
]
```

In the previous example, the result set contains the last data of 2 devices. As
these data are subject to change / enhancement, their description can be found
in the swagger documentation.

## Contact us

If you have any question, requirements, or suggestion, about the API or the
documentation itself, don't hesitate to contact us via email : 
[contact@api-k.com](mailto:contact@api-k.com?subject=About%20Geo%20API%20v2).

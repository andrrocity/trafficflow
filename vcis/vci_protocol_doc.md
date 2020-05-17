# Vehicle Control Interface Protocol Specification

Version 0.1

## Introduction

Transport is assumed to take place over a reliable, stream based connection, such as serial or TCP.  This protocol uses a request/response mechanism, similiar to that of HTTP.  There may be only one outstanding request, for either direction, at a time.

## Messages

A message consists of a start-line and zero or more header lines.  Each line is seperated by a `<CR><LF>` sequence.  The end of headers is indicated by `<CR><LF><CR><LF>`.

### Body Content

To indicate a messsage contains body content, a `Content-Length` and `Content-Type` header must be present.

### Requests

A request message consists of a verb, a request URI, and protocol/version information.

`
VERB /request/uri VCI/1.0
`

### Responses

A response message consists of a status code, and description.

`
VCI/1.0 200 OK
`

------------------------

## Request Verbs

The following verbs are supported:

`GET`
: Gets the resource at the given request URI.

`SUBSCRIBE`
: Subscribes for notifications when the value represented by the given request URI changes.

`UNSUBSCRIBE`
: Unsubscribes from notificaitons.  Use `*` for the request URI to unsubscribe from all.

`NOTIFY`
: Notifies the client that the value at the given URI has changed.  The body of this request may contain the updated value. *Note*: Notify requests do NOT need a response.

`PUT`
: Attempts to change the value at the given request URI with the one specified in the request body.

------------------------

## Status Code Classes

### **2xx Success Response**

`200 OK`
: Standard successful response code.

### **4xx Client Errors**

`400 Bad Request`
: The request was malformed.

### **5xx Server Errors**

`500 Internal Server Error`
: A generic error message, given when an unexpected condition was encountered and no more specific message is suitable

`501 Not Implemented`
: The server either does not recognize the request method, or it lacks the ability to fulfil the request.

------------------------

## Content Types

This specification's primary content-type is `json`.  This content-type does not require a `Content-Length` header, as the end of the body can be determined by parsing the JSON.

Any JSON value may be sent, even scalars such as booleans and numbers.  When sending a number, a single whitespace character must be sent to indicate the end of the number.

------------------------

## Units

All measurements must be represented in metric system as follows:

| Measuremen    | Units                                         |
| --------------| ----------------------------------------------|
| Length        | Meters (or `m`)                               |
| Speeed        | Meters per second (or `m/s`)                  |
| Acceleration  | Meters per second per second (or `m/s^2`)     |
| Torque        | Newton-meters (or `Nm`)                       |

---
author : "Sumit"
title : "HTTP vs HTTP 2.0"
date : "2023-04-14"
description : "What, Why and How of Context in Go"
tags : [
    "http",
    "backend",
]
summary: "This article explains how  the context package is used and how it works."
published : true
---

## What is HttP?

HTTP, or Hypertext Transfer Protocol, is an application-layer protocol for transmitting hypermedia documents, such as HTML. It is the foundation of any data exchange on the Web and it is a client-server protocol, which means requests are initiated by the recipient, usually the Web browser .


<!-- | Version   | Features
| ------------ | ---------------- |
| HTTP/0.9	| Simple, single-line requests and responses |
| HTTP/1.0	| Supports more methods, persistent connections, and caching |
| HTTP/1.1	| Improved performance with caching, pipelining, and compression |
| HTTP/2	| Multiplexing, header compression, and server push |
| HTTP/3	| Lossy network performance improvements | -->

## Evolution of HTTP

The Hypertext Transfer Protocol (HTTP) has evolved over time to improve performance, security, and features. Let's have a brief overview of the evolution of HTTP:

- __HTTP/0.9__: The first version of HTTP was released in 1991. It was a very simple protocol that only supported the GET - method and did not support persistent connections.
- __HTTP/1.0__: The second version of HTTP was released in 1996. It added support for more methods, such as POST, PUT, and DELETE, and it also supported persistent connections.
- __HTTP/1.1__: The third version of HTTP, released in 1997. It made a number of improvements to the protocol, including support for caching, pipelining, and compression.
- __HTTP/2__: The fourth version of HTTP, released in 2015. It is a major revision of the protocol that introduces a number of new features, such as multiplexing, header compression, and server push.
- __HTTP/3__: The fifth version of HTTP, still in development. It is designed to improve performance over lossy networks, such as the Internet.

HTTP/2 is still the most widely used version of HTTP today. It is supported by most web browsers and web servers. HTTP/3 is in development, but it is expected to become more widely used in the future.

## How HTTP Works

HTTP follows a classical client-server model, with a client opening a connection to make a request, then waiting until it receives a response. HTTP is a stateless protocol, meaning that the server does not keep any data (state) between two requests. However, some mechanisms, such as cookies and sessions, can be used to maintain state information across multiple requests .

![Client Server Model](/images/blogs/http_vs_http2.0/client_server_model.svg)

A typical HTTP session consists of the following steps:

- The client sends an HTTP request message to the server. The request message consists of a request line (containing the method, URI, and protocol version), optional request headers (containing additional information about the request), and an optional message body (containing data or query parameters) .
- The server receives the request and processes it. The server may perform various operations based on the request method, such as retrieving or modifying a resource, executing a script, or performing authentication or authorization. The server then sends an HTTP response message to the client. The response message consists of a status line (containing the protocol version, status code, and reason phrase), optional response headers (containing additional information about the response), and an optional message body (containing the requested resource or data) .
- The client receives the response and processes it. The client may display the resource or data to the user, store it in a cache, or perform further actions based on the status code or headers. The client may also send additional requests to the server if needed .

![http](/images/blogs/http_vs_http2.0/httpmsg.svg)


## What are the problems with HTTP?

HTTP is a widely used and successful protocol, but it also has some limitations and drawbacks that affect its performance and efficiency. Some of these problems are:

- __Head-of-line blocking__: Head of line blocking (HOL) is a performance-limiting phenomenon that occurs when a line of packets is held up by the first packet.
    
    In HTTP/1.1, this problem can happen when multiple requests are sent over a single TCP connection using pipelining, but the server has to send the responses in the same order as the requests. This means that if the first response is slow or delayed, all the subsequent responses are blocked behind it. This problem is exacerbated by the fact that modern web applications often require multiple resources (such as images, scripts, stylesheets, etc.) to render a page, which means that multiple TCP connections have to be opened and closed for each page load.
- __Redundant and verbose headers__: As mentioned before, HTTP 1.1 sends redundant and verbose header fields in each request and response, which increases the size of the messages and wastes bandwidth. This problem is worsened by the fact that some header fields (such as cookies) can be very large and contain sensitive information that should not be exposed or tampered with.
- __Unencrypted communication__: HTTP 1.1 does not provide any encryption or security mechanisms by default, which means that all requests and responses are sent in plain text over the network, which exposes them to eavesdropping, interception, modification or injection attacks. Instead HTTPS (HTTP over TLS) is used for secure communication, which adds encryption and authentication layers on top of HTTP. However, HTTPS also adds additional overhead and complexity to the protocol.

## What is HTTP 2.0?

Let's take a brief look at the history of HttP 2.0. 

HTTP 2.0 is a major revision of HTTP that was derived from an experimental protocol called SPDY, originally developed by Google. HTTP 2.0 was developed by the HTTP Working Group of the Internet Engineering Task Force (IETF) and published as RFC 7540 in May 2015.

HTTP 2.0 maintains high-level compatibility with HTTP 1.1, meaning that it uses the same methods, status codes, URIs and most header fields as HTTP 1.1. However, it introduces significant changes in how the data is framed and transported between the client and the server.


## How does HTTP 2.0 solvees the problem ?

HTTP 2.0 was designed keeping in mind the problems associated with HTTP 1.1. Let's see what changes HTTP 2.0 brough to address the problems.

- __Binary framing layer__: As opposed to HTTP 1.1, which keeps all requests and responses in plain text format, HTTP 2.0 uses a binary framing layer to encapsulate all messages in binary format, while still maintaining HTTP semantics. The binary framing layer divides each message into smaller units called frames, which have a fixed length header and a variable length payload. Each frame belongs to a stream, which is a bidirectional flow of frames between the client and the server that share a common identifier. Each stream has a priority and can be dependent on another stream.
- __Multiplexing__: Multiplexing is the ability to send and receive multiple requests and responses over a single connection, without waiting for each one to finish before starting the next one. This reduces the latency and overhead of establishing and closing multiple connections, and allows the browser and the server to use the network more efficiently.
    ![http vs http2.0](/images/blogs/http_vs_http2.0/multiplexing.svg)

    In HTTP/2, each request and response pair is associated with a unique identifier, called a __stream ID__, and is divided into smaller units, called __frames__. Frames can be sent and received in any order, as long as they have the correct stream ID. This allows multiple requests and responses to be multiplexed over a single connection, without blocking each other.
    It reduces the number of connections that need to be opened and maintained, which saves CPU and memory resources on both the browser and the server.
    It also reduces the latency of transferring multiple resources, especially for small or dependent resources, which improves the page load time and user experience.

- __Header compression__: Another problem of HTTP 1.1 is that it sends redundant and verbose header fields in each request and response, which increases the size of the messages and wastes bandwidth. To solve this problem, HTTP 2.0 uses a compression algorithm called HPACK to reduce the size of the header fields. HPACK uses a static table of common header fields and a dynamic table of header fields that are updated during the session. HPACK also employs Huffman encoding to further compress the header values.
- __Server push__: A feature of HTTP 2.0 that allows the server to send resources to the client before the client requests them, which can improve the performance of web applications. For example, if the client requests an HTML document that contains references to several images, the server can push those images to the client along with the HTML document, without waiting for the client to request them. This can reduce the number of round trips and save bandwidth. The client can accept or reject the pushed resources based on its cache or preferences.
- __Stream prioritization__: A feature of HTTP 2.0 that allows the client to indicate the relative priority of each stream, which can help the server to allocate resources and optimize bandwidth. For example, if the client requests an HTML document that contains references to several images and scripts, the client can assign higher priority to the scripts than to the images, so that the scripts can be executed faster and render the page sooner. The server can use this information to prioritize the delivery of the frames according to their stream priority and dependency.

So, in this article, we tried to understand the major problems associated with HTTP 1.1 and how HTTP 2.0 brought some solved those problems. Though I tried to address the most important concepts here, there is always scope for some more. So check out this [whitepaper by nginx](https://cdn-1.wp.nginx.com/wp-content/uploads/2015/09/NGINX_HTTP2_White_Paper_v4.pdf) for a deep dive.

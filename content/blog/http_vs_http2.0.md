---
author : "Sumit"
title : "HTTP vs HTTP 2.0"
date : "2023-02-05"
description : "What, Why and How of Context in Go"
tags : [
    "http",
    "backend",
]
summary: "This article explains how  the context package is used and how it works."
published : true
---

HTTP stands for Hypertext Transfer Protocol. It is the protocol that enables communication between different systems, transferring information and data over a network. HTTP is widely used on the web, as it allows web browsers and web servers to communicate and exchange web pages and other resources.


## A Detailed Blog About HTTP and Its Technical Aspects

HTTP, or Hypertext Transfer Protocol, is an application-layer protocol for transmitting hypermedia documents, such as HTML. It is the foundation of any data exchange on the Web and it is a client-server protocol, which means requests are initiated by the recipient, usually the Web browser .

## Evolution of HTTP

HTTP has evolved over time to meet the changing needs and demands of the Web. The original version of HTTP, HTTP/0.9, was a simple protocol that only supported GET requests and plain text responses. The next version, HTTP/1.0, introduced headers, methods, status codes, and MIME types to support different kinds of resources and data formats. HTTP/1.1, the most widely used version of HTTP, added features such as persistent connections, chunked encoding, pipelining, caching, compression, and conditional requests to improve performance, efficiency, and reliability. HTTP/2, the latest version of HTTP, introduced a binary framing layer, multiplexing, stream prioritization, server push, and header compression to further enhance speed, security, and flexibility. HTTP/3, the emergent version of HTTP, is based on QUIC, a new transport protocol that uses UDP instead of TCP and provides built-in encryption, congestion control, and error correction .

HTTP and HTTP 2.0 are two versions of the same application protocol used to exchange information on the World Wide Web. HTTP stands for Hypertext Transfer Protocol and it was first released in the early 1990s as a simple way to communicate between clients and servers over the Internet. HTTP 2.0 is a major revision of HTTP that was standardized in 2015 and aims to improve the performance, efficiency and security of web applications. In this blog post, we will explore the main differences between HTTP and HTTP 2.0, the problems that HTTP faces and how HTTP 2.0 tries to solve them, how both protocols are implemented and how to choose between them.

## How HTTP Works

HTTP follows a classical client-server model, with a client opening a connection to make a request, then waiting until it receives a response. HTTP is a stateless protocol, meaning that the server does not keep any data (state) between two requests. However, some mechanisms, such as cookies and sessions, can be used to maintain state information across multiple requests .

A typical HTTP session consists of the following steps:

- The client sends an HTTP request message to the server. The request message consists of a request line (containing the method, URI, and protocol version), optional request headers (containing additional information about the request), and an optional message body (containing data or query parameters) .
- The server receives the request and processes it. The server may perform various operations based on the request method, such as retrieving or modifying a resource, executing a script, or performing authentication or authorization. The server then sends an HTTP response message to the client. The response message consists of a status line (containing the protocol version, status code, and reason phrase), optional response headers (containing additional information about the response), and an optional message body (containing the requested resource or data) .
- The client receives the response and processes it. The client may display the resource or data to the user, store it in a cache, or perform further actions based on the status code or headers. The client may also send additional requests to the server if needed .


## What are the problems with HTTP?

HTTP is a widely used and successful protocol, but it also has some limitations and drawbacks that affect its performance and efficiency. Some of these problems are:

- __Head-of-line blocking__: Head of line blocking (HOL) is a performance-limiting phenomenon that occurs when a line of packets is held up by the first packet. In HTTP/1.1, this problem can happen when multiple requests are sent over a single TCP connection using pipelining, but the server has to send the responses in the same order as the requests. This means that if the first response is slow or delayed, all the subsequent responses are blocked behind it. This problem is exacerbated by the fact that modern web applications often require multiple resources (such as images, scripts, stylesheets, etc.) to render a page, which means that multiple TCP connections have to be opened and closed for each page load. This also increases the overhead of TCP handshakes and slow start mechanisms.
- __Redundant and verbose headers__: As mentioned before, HTTP 1.1 sends redundant and verbose header fields in each request and response, which increases the size of the messages and wastes bandwidth. This problem is worsened by the fact that some header fields (such as cookies) can be very large and contain sensitive information that should not be exposed or tampered with.
- __Unencrypted communication__: HTTP 1.1 does not provide any encryption or security mechanisms by default, which means that all requests and responses are sent in plain text over the network, which exposes them to eavesdropping, interception, modification or injection attacks. To secure HTTP communication, HTTPS (HTTP over TLS) is often used, which adds encryption and authentication layers on top of HTTP. However, HTTPS also adds additional overhead and complexity to the protocol.


## What is HTTP 2.0?

HTTP 2.0 is a major revision of HTTP that was derived from an experimental protocol called SPDY, originally developed by Google. HTTP 2.0 was developed by the HTTP Working Group of the Internet Engineering Task Force (IETF) and published as RFC 7540 in May 2015.

HTTP 2.0 maintains high-level compatibility with HTTP 1.1, meaning that it uses the same methods, status codes, URIs and most header fields as HTTP 1.1. However, it introduces significant changes in how the data is framed and transported between the client and the server.

### The main features of HTTP 2.0 are:

- Binary framing layer: As opposed to HTTP 1.1, which keeps all requests and responses in plain text format, HTTP 2.0 uses a binary framing layer to encapsulate all messages in binary format, while still maintaining HTTP semantics. The binary framing layer divides each message into smaller units called frames, which have a fixed length header and a variable length payload. Each frame belongs to a stream, which is a bidirectional flow of frames between the client and the server that share a common identifier. Each stream has a priority and can be dependent on another stream.
- Multiplexing: Multiplexing is the ability to send and receive multiple requests and responses over a single connection, without waiting for each one to finish before starting the next one. This reduces the latency and overhead of establishing and closing multiple connections, and allows the browser and the server to use the network more efficiently.

    In HTTP/2, each request and response pair is associated with a unique identifier, called a **stream ID**, and is divided into smaller units, called **frames**. Frames can be sent and received in any order, as long as they have the correct stream ID. This allows multiple requests and responses to be multiplexed over a single connection, without blocking each other.
    It reduces the number of connections that need to be opened and maintained, which saves CPU and memory resources on both the browser and the server.
    It also reduces the latency of transferring multiple resources, especially for small or dependent resources, which improves the page load time and user experience.

- Header compression: Another problem of HTTP 1.1 is that it sends redundant and verbose header fields in each request and response, which increases the size of the messages and wastes bandwidth. To solve this problem, HTTP 2.0 uses a compression algorithm called HPACK to reduce the size of the header fields. HPACK uses a static table of common header fields and a dynamic table of header fields that are updated during the session. HPACK also employs Huffman encoding to further compress the header values.
- Server push: A feature of HTTP 2.0 that allows the server to send resources to the client before the client requests them, which can improve the performance of web applications. For example, if the client requests an HTML document that contains references to several images, the server can push those images to the client along with the HTML document, without waiting for the client to request them. This can reduce the number of round trips and save bandwidth. The client can accept or reject the pushed resources based on its cache or preferences.
- Stream prioritization: A feature of HTTP 2.0 that allows the client to indicate the relative priority of each stream, which can help the server to allocate resources and optimize bandwidth. For example, if the client requests an HTML document that contains references to several images and scripts, the client can assign higher priority to the scripts than to the images, so that the scripts can be executed faster and render the page sooner. The server can use this information to prioritize the delivery of the frames according to their stream priority and dependency.

## How does HTTP 2.0 solve these problems?



- __Head-of-line blocking__: HTTP/2 solves this problem by introducing __multiplexing__, which allows multiple requests and responses to be sent over the same connection __concurrently__ and __independently__. Multiplexing uses binary framing to split the HTTP messages into smaller frames that can be interleaved and reassembled at the end points. This way, the server can send the responses as soon as they are ready, without waiting for the previous ones to complete. Multiplexing also reduces the overhead of opening and closing multiple connections, and enables more efficient use of network resources.

    However, HTTP/2 still suffers from HOL blocking at the TCP layer, because TCP is a reliable and ordered protocol that requires the packets to be delivered in sequence. If a packet is lost or corrupted, all the subsequent packets have to wait until the retransmission of the missing packet. This affects all the HTTP/2 streams that share the same TCP connection. To overcome this limitation, a new protocol called QUIC (Quick UDP Internet Connections) is being developed, which implements a TCP-like protocol over UDP, where each stream is independent and can recover from packet loss without blocking other streams.

- __Stream prioritization__: It is a feature of HTTP/2 that allows clients and servers to indicate the relative importance of different resources, such as HTML, CSS, JavaScript, images, etc. By using stream prioritization, clients and servers can optimize the use of the underlying TCP connection and improve the perceived performance of web pages.

    HTTP/2 is a binary protocol that multiplexes multiple requests and responses over a single TCP connection. Each request and response is divided into frames, which are assigned to a stream. A stream is a bidirectional flow of frames that share a common identifier and have some associated state. Each stream can carry one or more messages, which are logical sequences of frames that represent a single request or response.

    Stream prioritization is implemented by using two types of frames: HEADERS and PRIORITY. The HEADERS frame contains the HTTP headers of a request or response, and optionally some priority information. The PRIORITY frame can be sent at any time to update the priority information of a stream. The priority information consists of three fields:
    - __Stream dependency__: an identifier of another stream that this stream depends on. This creates a dependency tree of streams, where the root stream has no dependency and the leaf streams have no dependents.
    - Weight: an integer between 1 and 256 that indicates the relative weight of this stream compared to its siblings. Higher weights are more important.
    - __Exclusive__: a boolean flag that indicates whether this stream should be the only dependent of its parent stream.

    The dependency tree and the weights are used by the server to determine the order and proportion of data to send for each stream. The server should try to satisfy the dependencies of higher-priority streams before lower-priority ones, and allocate more bandwidth to higher-weight streams than lower-weight ones. The exclusive flag can be used to rearrange the dependency tree and give a stream higher or lower priority.

    For example, suppose a client requests a web page that consists of an HTML document, a CSS file, a JavaScript file, and two images. The client can use stream prioritization to indicate that the HTML document is the most important resource, followed by the CSS file, then the JavaScript file, and finally the images. The client can also indicate that the CSS file depends on the HTML document, and the JavaScript file depends on both the HTML document and the CSS file. The client can assign different weights to each resource according to their size or importance.

- __Header compression__: It is a technique that reduces the size of HTTP headers, which are used to exchange information between clients and servers. HTTP headers can contain many fields, such as cookies, user-agent, content-type, etc., that are often repeated across multiple requests and responses. This can result in a lot of overhead and slow down the web performance, especially on mobile devices.

    HTTP 2.0 introduces a new header compression algorithm, called HPACK, that is designed to address this problem. HPACK uses three methods of compression:

    - __Static table__: A predefined list of common header fields and values that are assigned fixed indexes. For example, the field :method with the value ```GET``` has the index 2.
    - **Dynamic table**: A list of header fields and values that are dynamically updated based on the headers seen in previous requests and responses. The dynamic table is maintained by both the client and the server and synchronized using index values.
    - **Huffman encoding**: A variable-length coding scheme that assigns shorter codes to more frequent symbols and longer codes to less frequent symbols. For example, the ASCII character 'e' has the code `1110`, while the character `z` has the code `101011`.

    Using these methods, HPACK can compress headers by eliminating redundancy, using shorter representations, and exploiting the context of previous headers. For example, if a client sends a request with the header `:method: GET`, it can use the index 2 to refer to it. If the server responds with the same header, it can omit it entirely, since it is already in the dynamic table. If a header field or value is not in the static or dynamic table, it can be encoded using Huffman codes.

    Header compression in HTTP 2.0 can significantly improve the web performance by reducing the bandwidth usage, latency, and packet loss. It can also enhance the security and privacy of HTTP communication by preventing attacks like CRIME that exploit the compression of sensitive data.

- HTTP/2 is the latest version of the HTTP protocol that powers the web. It offers many benefits, such as faster page loading, server push, and multiplexing. But how does encryption work in HTTP/2?

    HTTP/2 can be used with or without encryption, but most browsers only support it over TLS, the protocol that secures HTTPS connections. To use HTTP/2 with encryption, servers and clients need to negotiate the use of the protocol using a mechanism called ALPN (Application-Layer Protocol Negotiation). ALPN allows servers to indicate which protocols they support, and clients to choose the best one for them. For example, a server can say that it supports HTTP/1.1, HTTP/2, and SPDY, and a client can pick HTTP/2 as the preferred option.

    When a client connects to a server using TLS, it sends a list of supported protocols in the ClientHello message. The server then responds with the selected protocol in the ServerHello message. The rest of the TLS handshake proceeds as usual, and the server presents a certificate for the domain signed by a trusted certificate authority. The client validates the certificate and establishes a secure connection. Then, the client and server can exchange HTTP/2 messages over that connection.

    However, not all websites use HTTPS or have valid certificates. Some websites still use HTTP, which is unencrypted and vulnerable to eavesdropping and tampering. To enable HTTP/2 for these websites, there is another mechanism called Opportunistic Encryption. Opportunistic Encryption allows servers to tell clients that they can accept HTTP requests over an encrypted connection, even if they don't have a valid certificate.

    To use Opportunistic Encryption, servers need to send an Alternative Service header to clients, indicating that they support HTTP/2 (or SPDY) over TLS on port 443. For example:

    Alt-Svc: h2=":443"; ma=60

    This header tells the client that the website can be accessed using HTTP/2 over TLS on port 443 for 60 seconds. The client then tries to connect to the server using TLS on port 443, where the server presents a certificate for the domain. The client does not validate the certificate, but only checks that it matches the domain name. If the connection succeeds, the client sends HTTP requests over that connection using HTTP/2. However, these requests are not considered HTTPS requests, but HTTP requests over an encrypted connection. The client does not show any security indicator in the address bar for these requests.

    Opportunistic Encryption provides some level of security and performance benefits for websites that have not yet migrated to HTTPS. However, it is not a substitute for HTTPS, as it does not guarantee authenticity or integrity of the connection. Opportunistic Encryption is only supported by some browsers, such as Firefox, and is enabled by default for CloudFlare customers.

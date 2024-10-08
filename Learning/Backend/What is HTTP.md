#### What is a protocol
A **protocol** is a system of rules that define how data is exchanged within or between computers. Communications between devices require an agreed upon format of the data that is being exchanged. The set of rules that defines a format is called a protocol.

Source: *[MDN](https://developer.mozilla.org/en-US/docs/Glossary/Protocol)*

---
#### What is HTTP
Hypertext Transfer Protocol (HTTP) is the foundation of the World Wide Web, and is used to load webpages using hypertext links.
HTTP is an *application layer* protocol designed to transfer information between networked devices and runs on top of the network *protocol* stack. A typical flow over HTTP involves a client machine making a request to a server, which then sends a response message.

#### What is an HTTP request?
An HTTP request is the way Internet communication platforms (such as web browsers) ask for the information they need to load a website.
HTTP requests carry encoded data, and typically contains:
- HTTP version type
- URL
- HTTP method (GET, POST, PUT, etc.)
- HTTP request headers (key-value pairs of core text information)
- Optional HTTP body (any information being submitted to the web server)

#### What is an HTTP response?
An HTTP response is what web clients receive from an Internet server in answer to an HTTP request. HTTP responses typically contain:
- HTTP status code (3-digit code indicating whether request was successful)
- HTTP response headers (similar to HTTP request's headers, convey important information such as language and format of data)
- Optional HTTP body (e.g. HTML data, JSON)

#### Can DDoS attacks be launched over HTTP?
HTTP is a "stateless" protocol thus each command runs independent of any other command. Newer HTTP protocols use persistent TCP connection, which means that multiple HTTP requests can pass through it, improving resource consumption.
HTTP requests in large quantities can be used to mount an attack on a target device, and are considered part of application layer (or layer 7) attacks.

Source: *[Cloudflare](https://www.cloudflare.com/en-gb/learning/ddos/glossary/hypertext-transfer-protocol-http/)* 

---
#### Client-Server communication
Clients and servers communicate by exchanging individual messages (as opposed to a stream of data). The messages sent by client are called requests and the messages sent by server as an answer -- responses.

#### How is HTTP protocol sent?
HTTP is sent over TCP, or over TLS-encrypted TCP connection, though any reliable transport protocol could theoretically be used.

##### What is TCP
**Transmission Control Protocol (TCP)** is an network protocol that lets two hosts connect and exchange data streams. TCP guarantees the delivery of data packets in the same order as they were sent.
TCP's role is to ensure the packets are reliably delivered, and error-free. It implements congestion control, which means that initial requests start small, increasing in size to the levels of bandwidth the computers, servers, and network can support.

##### What is TLS
**Transport Layer Security (TLS)**, formerly known as **Secure Sockets Layer (SSL)**, is a protocol used by applications to communicate securely across a network, preventing tampering with and eavesdropping on email, web browsing, messaging, and other protocols. Both TLS and SSL are client / server protocols that ensure communication privacy by using cryptographic protocols.
All modern browsers support TLS, requiring the server to provide a valid digital certificate confirming its identity in order to establish a secure connection. It is possible for both the client and the server to mutually authenticate each other, if both parties provide their own individual digital certificates.

#### Components of HTTP-based systems
Requests are sent by one entity -- the user-agent (or a proxy on behalf of it). Most of the time user-agent is a Web browser, but it can be anything.
The server handles the request and provides a response. Between the client and the server there are numerous entities, collectively called **proxies**, which perform different operations and act as gateways or caches, for example.
Other devices, such as routers, modems, and more are hidden in the network and transport layers. These underlying layers are mostly irrelevant to the description of HTTP.

The browser is **always** the entity initiating the request.

##### Proxies
Proxies are computers or machines that relay HTTP messages at the application layer. They can be transparent (forwarding on the requests they receive without altering them), or non-transparent (changing request in some way before passing it along to the server). Proxies may perform numerous functions:
- caching (public or private, like the browser cache)
- filtering (antivirus scan or parental controls)
- load balancing (allows multiple servers to serve different requests)
- authentication (controls access to different resources)
- logging (storage of historical information)

##### Basic aspects of HTTP
- HTTP is simple - it is generally designed to be human-readable.
- HTTP is extensible - HTTP headers allow extension of the protocol.
- HTTP is stateless, but not sessionless - while there is no link between to requests being successively carried out in the same connection, HTTP cookies, using header extensibility, allow the use of stateful sessions.
- HTTP and connections - a connection is controlled at the transport layer, thus it is outside of scope for HTTP. HTTP only requires the transport to be reliable, or not lose messages. Google is experimenting with QUIC which builds on UDP to provide a more reliable and efficient transport protocol.

##### What can be controlled by HTTP
Common features controllable by HTTP are:
- Caching (how documents are cached). The server can instruct proxies and clients about what to cache and for how long. The client can instruct intermediate cache proxies to ignore the stored document.
- Relaxing the origin constraint (added in 2010s). To prevent snooping and other privacy invasions, web browsers enforce strict separation between websites. Only pages of the same origin can access all the information of a Web page. HTTP headers can relax this strict separation on the server side, allowing a document to be sourced from different domains.
- Authentication: Some pages may be protected so that only specific users can access them.
- Proxy and tunneling: Servers and clients are often located on intranets and hide their true IP address from other computers. HTTP requests go through proxies to cross this network barrier.
- Sessions: Using HTTP cookies allows linking of requests with the state of the server.

##### HTTP flow
1. Open a TCP connection
2. Send an HTTP message
3. Read the response sent by server
4. Close or reuse connection for further requests.
If HTTP pipelining is activated, several requests can be sent without waiting for the first response to be fully received. It has been superseded in HTTP/2 with more robust multiplexing requests within a frame.

Source: *[MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)*

---

#### HTTP versions
**HTTP/1** - every request requires separate TCP connection
**HTTP/1.1** - introduces "keep-alive" to reuse the same TCP connection. Also introduced HTTP pipelining, allowing the client to send multiple requests before waiting for the response. Each response needed to be sent in order. It was not handled properly, and support for pipelining was later removed.
**HTTP/2** - "streams" compressed headers. Multiple streams of requests can be sent to the same server on the same TCP connection. Unlike HTTP/1.1 each stream is independent, and does not need to be sent/received in order. It also introduced "push" capability, to allow servers to send information without requiring a client to "pull".
**HTTP/3** - uses new protocol called **QUIC** (based on UDP) instead of TCP. QUICK stream can handle multiple connections, packet loss on one stream does not affect others. It is designed for mobile heavy uses -- switching network (e.g. 5G/4G/WIFI) is tackled by QUICK connection ID which allows connections to move quickly between IP addresses and network interfaces quickly and reliably.

**More on HTTP/3**
As of September 2024, HTTP/3 is supported by 95% of major web browsers (though not enabled for all Safari users) and 31% of the top 10 million websites. *[Source](https://en.wikipedia.org/wiki/HTTP/3)*

More on **HTTP/3**: https://www.smashingmagazine.com/2021/08/http3-core-concepts-part1/

##### SSL, TLS, HTTPS
**HTTPS** (Hypertext Transfer Protocol Secure) is an encrypted version of HTTP, using TLS (Transport Layer Security) to protect data. This ensures that if data is intercepted, it appears scrambled and unreadable.
##### How it works:
1. **TCP Handshake**: The browser establishes a TCP connection with the server, just like with HTTP.
2. **Certificate Exchange**: The browser sends a "Client Hello" message, specifying supported TLS versions and encryption algorithms. The server replies with a "Server Hello," selecting the appropriate options. The server also sends its digital certificate, including its public key, for use in encryption.
3. **Key Exchange**: The client generates a session key and encrypts it using the server's public key (asymmetric encryption). It sends the encrypted session key to the server, which decrypts it using its private key. Now both sides share the session key.
4. **Data Transmission**: Both the client and server use the session key to encrypt and decrypt the data exchanged between them. This ensures secure communication.
###### Asymmetric encryption
Asymmetric encryption uses two keys: a public key to encrypt data and a private key to decrypt it. Only the private key holder can decrypt what was encrypted with the public key, ensuring secure communication.
Asymmetric encryption is computationally expensive, thus it is not used for everything.
#### What is a web browser?
A web browser is a software program that enables a user to access information on the Internet via the World Wide Web. It acts like a translator, taking information from web servers and displaying it to the user as a web page.

![Schema of web browser components](../../Note%20Pictures/Pasted%20image%2020241008215723.png)

#### Browser Components
Some key components of browsers include:
- **User Interface (UI)** - you interact with it directly. It includes the address bar, tabs, etc.
- **Rendering Engine** - the architect responsible for building the visual representation of the webpage.
- **Networking Component** - it fetches website files from web servers, ensuring all necessary pieces are delivered to the rendering engine.
- **JavaScript Engine** - it interprets and executes JavaScript code.
- **Security Components** -- they handle tasks like encrypting data transmissions (HTTPS) and protecting you from malicious websites.

#### Rendering Engine - The Web Architecture
It is the main component of a web browser that converts the website code into visual elements on a web page. The rendering engine receives instructions from a browser, interprets them, and builds a webpage accordingly.
Following are the rendering engines used by mainstream browsers:
- **Blink** - this engine powers Google Chrome and is known for its speed and efficiency.
- **WebKit** - used in Safari and other browsers, WebKit offers robust features for rendering complex webpages.
- **Gecko** - the engine behind Mozilla Firefox prioritizes open-source development and standards compliance.

#### Types of web browsers
- **Desktop Browsers** - productivity powerhouses, offering complete features and functions for a smooth browsing experience.
- **Mobile Browsers** -- designed for smaller screens and touch interfaces. Usability and fast loading times are key features of mobile browsers.
- **Embedded Browsers** - miniature versions of web browsers embedded into other applications, with limited features. Embedded browsers are commonly used for email clients, social media apps, gaming consoles.

Source: *[Ramotion](https://www.ramotion.com/blog/what-is-web-browser/)*

---
#### Performance
Two major issues in web performance are issues having to do with latency and issues having to do with the fact that for the most part, browsers are single-threaded.

Latency is the biggest threat to our ability to ensure a fast-loading page. Network latency is the time it takes to transmit bytes over the air to computers. Web performance is what we have to do to make the page load as quickly as possible.

For the most part, browsers are considered single-threaded: they execute a task from beginning to end before taking up another task. For smooth interactions, the developer's goal is to ensure performant site interactions, from smooth scrolling to being responsive to touch.
Render time is key, ensuring the main thread can complete all the work we throw at it and still always be available to handle user interactions.
Web performance can be improved by minimizing the main thread's responsibilities, where possible and appropriate.

#### Navigation
Navigation is the first step in loading a web page. It occurs whenever a user request a page by entering a URL, clicking a link, submitting a form, etc.

One of the goals of web performance is to minimize the amount of time navigation takes to complete. In ideal conditions, this doesn't take too long, but latency and bandwidth can cause delays.

##### DNS lookup
The first step of navigating to a web page is finding where the assets for the page are located.
The browser requests a DNS lookup, after which the IP will likely be cached for a time, which speeds up subsequent requests.

More on DNS: [What is DNS](What%20is%20DNS.md)
##### TCP handshake
Once the IP address is known, the browser sets up a connection to the server via a TCP three-way handshake. This mechanism is designed so that two entities attempting to communicate can negotiate the parameters of the network TCP socket connection before transmitting data, ofter over HTTPS.

TCP's three-way handshaking technique is often referred to as "SYN-SYN-ACK" -- or more accurately SYN, SYN-ACK, ACK -- because there are three messages transmitted by TCP to negotiate and start a TCP session between two computers.

1. The initiator, generally the browser, sends a TCP SYNchronize packet to the other host, generally the server.
2. The server receives the SYN and sends back a SYNchronize-ACKnowledgement.
3. The initiator receives the server's SYN-ACK and sends an ACKnowledge. The server receives ACK and the TCP socket connection is established.

##### TLS negotiation
For secure connections established over HTTPS, another "handshake" is required. This handshake (TLS negotiation) determines which cipher will be used to encrypt the communication, verifies the server, and establishes that a secure connection is in place before beginning the actual transfer of data. This requires five more round trips to the server before the request for content is actually sent.

![TLS negotiation](../../Note%20Pictures/Pasted%20image%2020241008222251.png)

More: TBD

Source: *[MDN](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work#tls_negotiation)*
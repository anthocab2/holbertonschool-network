# What Happens When You Type https://www.google.com in Your Browser and Press Enter?

The address bar makes visiting a website look like one action. Behind that action, your browser must find a destination, establish a secure connection, request a resource, and turn the response into a page you can use.

Let's follow that journey from the network to the application. We will use a fresh HTTPS connection over TCP as the main example, then explain where HTTP/3 changes the transport. The server-side examples describe common architectural roles; they are not a claim that Google's production system uses one particular web server or database.

## 1. The browser understands the address

The browser parses the URL into a scheme, a hostname, and a path. Here, `https` requests an encrypted HTTP connection, `www.google.com` identifies the host, and the omitted path becomes `/`. HTTPS uses port `443` by default.

At this point, we are asking for Google's home page. We have not submitted a search query. Cached resources or an existing connection may shorten the journey, but following a new connection makes each responsibility easier to see. [MDN: How the web works](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works)

## 2. DNS finds an IP address

The Domain Name System (DNS) translates a hostname into addresses the network can use. The browser and operating system may already have a valid cached answer. Otherwise, a DNS query goes to the configured recursive resolver, which may also answer from its cache.

Without the relevant cached information, the resolver follows a chain of referrals: a root name server points toward `.com`, the `.com` name servers identify the authoritative servers for `google.com`, and an authoritative server supplies the requested answer. An `A` record contains an IPv4 address; an `AAAA` record contains an IPv6 address. A `CNAME` alias, if present, requires resolving its target.

The result returns to the client and can be cached according to its time to live (TTL). DNS returns addressing information, not the website's HTML. Different clients can receive different valid addresses; there is no single permanent Google IP to memorize. [Cloudflare: DNS](https://www.cloudflare.com/learning/dns/what-is-dns/)

## 3. TCP/IP carries the conversation

The operating system sends IP packets toward the chosen address through the local network, the router, and other networks. Routers forward packets toward their destination using routing information. IP provides addressing and delivery across networks; it does not itself guarantee arrival or order. [MDN: How the web works](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works)

For our TCP example, the client uses a temporary source port and connects to destination port `443`. A new TCP connection normally starts with three messages:

1. The client sends `SYN` to begin the connection.
2. The server replies with `SYN-ACK`.
3. The client acknowledges with `ACK`.

TCP then provides a reliable, ordered byte stream. Sequence numbers, acknowledgments, and retransmissions handle missing or reordered data; flow and congestion control regulate how quickly it is sent. An unrecoverable network failure can still break the connection. TCP provides transport reliability, not encryption. [IETF: TCP, RFC 9293](https://www.rfc-editor.org/rfc/rfc9293.html)

## 4. Firewalls decide which traffic is allowed

Along the route and at the destination, firewalls enforce traffic rules. A network firewall may filter by source and destination IP, transport protocol, port, and connection state. For example, a service can allow incoming TCP traffic to its HTTPS endpoint while keeping internal database ports inaccessible from the public Internet.

This filtering happens as packets arrive, including during connection establishment. It is not a separate conversation that always starts after TCP finishes. A stateful firewall can recognize return traffic belonging to an allowed connection. These are examples of firewall policy, not a description of Google's private rules. [Cloudflare: Firewalls](https://www.cloudflare.com/learning/security/what-is-a-firewall/)

## 5. HTTPS uses TLS to secure the connection

With TCP established, the browser and HTTPS endpoint negotiate Transport Layer Security (TLS). People still say "SSL certificate," but modern HTTPS uses TLS, the successor to SSL. Its goals include authenticating the server, keeping the HTTP exchange confidential, and detecting modification in transit. [MDN: Transport Layer Security](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Transport_Layer_Security)

In a full, certificate-authenticated TLS 1.3 handshake, the client offers supported parameters and a key share. The server selects compatible parameters, supplies its key share, and sends its certificate chain and proof that it controls the corresponding private key. Both sides derive shared traffic keys; those keys are not sent across the network as plaintext. Finished messages verify the handshake. Symmetric authenticated encryption then protects the application data. [IETF: TLS 1.3, RFC 8446](https://www.rfc-editor.org/rfc/rfc8446.html)

The browser checks that the certificate is valid for `www.google.com`, is within its validity period, and chains to a trusted authority. A certificate error can stop normal navigation. HTTPS protects the exchange with the TLS endpoint; it does not make every action performed by a website trustworthy. [MDN: Transport Layer Security](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Transport_Layer_Security)

### What if the browser uses HTTP/3?

The TCP sequence above applies to HTTPS over TCP, including HTTP/1.1 and HTTP/2. HTTP/3 uses QUIC over UDP, with TLS 1.3 integrated into connection establishment, so it does not perform a TCP handshake. The browser may discover and use an HTTP/3 endpoint when supported. If QUIC connectivity fails, it can try a TCP-based HTTP version. The request still has the same HTTP meaning. [IETF: HTTP/3, RFC 9114](https://www.rfc-editor.org/rfc/rfc9114.html)

## 6. The HTTP request reaches a load balancer

Over the secure connection, the browser requests `/` from `www.google.com`. For readability, an HTTP/1.1 version would begin like this:

```http
GET / HTTP/1.1
Host: www.google.com
```

HTTP/2 and HTTP/3 encode the request differently. The example shows its meaning, not a packet capture of a Google visit. [MDN: How the web works](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works)

A large service distributes traffic across many machines. A load balancer selects a backend using routing policy, capacity, and availability information. This helps spread work and avoid unavailable destinations. Google's published SRE material describes multiple levels of load balancing, including DNS-based distribution and frontend traffic handling. [Google SRE: Load Balancing at the Frontend](https://sre.google/sre-book/load-balancing-frontend/)

The load balancer's position relative to TLS depends on the design. A transport-layer balancer can forward a connection without decrypting HTTP. An HTTPS reverse proxy can terminate the browser's TLS connection, inspect the request, and open a separate protected connection to a backend. It does not automatically extend the browser's TLS session to every internal server. [HAProxy: Enable TLS](https://www.haproxy.com/documentation/haproxy-configuration-tutorials/security/ssl-tls/basics-enable-tls/)

## 7. The web server handles HTTP

The web server receives HTTP requests and returns HTTP responses. It can serve static resources such as images, stylesheets, and JavaScript, or forward requests that require application logic to an application server.

For example, Nginx can serve a stored image directly and proxy a dynamic route to another process. This illustrates the role; it does not identify the software serving Google's home page. A cached response can also avoid some downstream work. [Nginx: Beginner's Guide](https://nginx.org/en/docs/beginners_guide.html)

## 8. The application server runs the logic

An application server executes the code that decides what a request means and what response to build. A typical application might validate inputs, interpret session information, choose a language, call other services, or retrieve stored data. It then produces HTML or another response format.

The web server and application server are logical responsibilities. They may run in separate processes or machines, and some software handles both roles. Loading a home page and processing a submitted search are different requests, so they can follow different application paths. [MDN: Introduction to the server side](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/First_steps/Introduction)

## 9. Databases and indexes supply stored information

A database stores and retrieves persistent information. In a typical application, this could include accounts, preferences, or content. The application communicates with the data layer and constructs a response; the browser does not normally connect directly to the private database. Cached or static content may need no database query at all. [MDN: Introduction to the server side](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/First_steps/Introduction)

For Google Search, the relevant distinction is between opening the home page and later submitting a query. Google documents a pipeline of crawling, indexing, and serving results. When a search is submitted, its systems consult an existing index to identify relevant pages; they do not crawl the whole web anew for that request. Google's documentation describes the index as a large database distributed across many computers. This does not establish which storage operation, if any, a particular home-page request performs. [Google Search Central: How Search Works](https://developers.google.com/search/docs/fundamentals/how-search-works)

## 10. The response becomes a visible page

The response travels back through the serving infrastructure to the browser. It includes a status, headers, and, when appropriate, a body. A successful document response can contain HTML; redirects or errors lead to different behavior.

The browser parses HTML into the Document Object Model (DOM), processes CSS, and fetches referenced resources as needed. JavaScript can change the document and trigger further requests. The browser calculates layout, paints content, and composites layers so the page appears on screen. Parsing, resource loading, and rendering can overlap. [MDN: How browsers work](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/How_browsers_work)

Pressing Enter starts a chain of distinct responsibilities: DNS finds addresses, transport moves data, firewalls enforce access rules, TLS protects the exchange, and servers produce the requested content. Following those boundaries also helps diagnose failures: a missing DNS answer, a blocked port, a rejected certificate, and a failing application are different problems even when each leaves the user without a working page.

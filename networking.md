## TCP vs UDP

| TCP                                                                                                                                                                                                                                        | UDP                                                                                                                                                                                                                              | 
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| Reliable: TCP is connection-oriented protocol. When a file or message send it will get delivered unless connections fails. If connection lost, the server will request the lost part. There is no corruption while transferring a message. | Not Reliable: UDP is connectionless protocol. When you a send a data or message, you don’t know if it’ll get there, it could get lost on the way. There may be corruption while transferring a message.                          | 
| Ordered: If you send two messages along a connection, one after the other, you know the first message will get there first. You don’t have to worry about data arriving in the wrong order.                                                | Not Ordered: If you send two messages out, you don’t know what order they’ll arrive in i.e. no ordered                                                                                                                           | 
| Heavyweight: – when the low level parts of the TCP “stream” arrive in the wrong order, resend requests have to be sent, and all the out of sequence parts have to be put back together, so requires a bit of work to piece together.       | Lightweight: No ordering of messages, no tracking connections, etc. It’s just fire and forget! This means it’s a lot quicker, and the network card / OS have to do very little work to translate the data back from the packets. | 
| Streaming: Data is read as a “stream,” with nothing distinguishing where one packet ends and another begins. There may be multiple packets per read call.                                                                                  | Datagrams: Packets are sent individually and are guaranteed to be whole if they arrive. One packet per one read call.                                                                                                            | 
| Examples: World Wide Web (Apache TCP port 80), e-mail (SMTP TCP port 25 Postfix MTA), File Transfer Protocol (FTP port 21) and Secure Shell (OpenSSH port 22) etc.                                                                         | Examples: Domain Name System (DNS UDP port 53), streaming media applications such as IPTV or movies, Voice over IP (VoIP), Trivial File Transfer Protocol (TFTP) and online multiplayer games                                    | 

## HTTP 
### Status code 
#### Groups 

| Status code | Meaning      | Examples                                                                      | 
|-------------|--------------|-------------------------------------------------------------------------------| 
| 5XX         | Server error | 500 Server Error                                                              | 
| 4XX         | Client error | 401 Authentication failure; 403 Authorization failure; 404 Resource not found | 
| 3XX         | Redirect     | 301 Resource moved permanently; 302 Resource moved temporarily                | 
| 2XX         | Success      | 200 OK; 201 Created; 203 Object marked for deletion                           | 

#### HTTP 4XX status codes 

| Status code | Meaning              |  Examples     | 
|-------------|----------------------|---------------| 
| 400         | Malformed request    | Frequently a problem with parameter formatting or missing headers | 
| 401         | Authentication error | The system doesn't know who the request if from. Authentication signature errors or invalid credentials can cause this | 
| 403         | Authorization error  | The system knows who you are but you don't have permission for the action you're requesting | 
| 404         | Page not found       | The resource doesn't exist | 
| 405         | Method not allowed   | Frequently a PUT when it needs a POST, or vice versa. Check the documentation carefully for the correct HTTP method | 


### Verbs 
#### CRUD example with Starbucks 

| Action          | System call                | HTTP verb address | Request body                                                                       | Successful response code + Response body                                                        | 
|-----------------|----------------------------|-------------------|------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------| 
| Order iced team | Add order to system        | Post /orders/     | {"name" : "iced tea", "size" : "trenta"}                                           | 201 Created Location: /orders/1                                                                 | 
| Update order    | Update existing order item | PUT /orders/1     | {"name" : "iced tea", "size" : "trenta", "options" : ["extra ice", "unsweetened"]} | 204 No Content or 200 Success                                                                   | 
| Check order     | Read order from system     | GET /orders/1     |                                                                                    | 200 Success { "name" : "iced tea", "size" : "trenta", "options" : ["extra ice", "unsweetened"]} | 
| Cancel order    | Delete order from system   | DELETE /orders/1  |                                                                                    | 202 Item Marked for Deletion or 204 No Content                                                  | 

* What about actions that don't fit into the world of CRUD operations?
	- Restructure the action to appear like a field of a resource. This works if the action doesn't take parameters. For example an activate action could be mapped to a boolean activated field and updated via a PATCH to the resource.
	- Treat it like a sub-resource with RESTful principles. For example, GitHub's API lets you star a gist with PUT /gists/:id/star and unstar with DELETE /gists/:id/star.
	- Sometimes you really have no way to map the action to a sensible RESTful structure. For example, a multi-resource search doesn't really make sense to be applied to a specific resource's endpoint. In this case, /search would make the most sense even though it isn't a resource. This is OK - just do what's right from the perspective of the API consumer and make sure it's documented clearly to avoid confusion.

#### Others 
* Put is a full update on the item. Patch is a delta update.
* Head: A lightweight version of GET
* Options: Discovery mechanism for HTTP
	- Link/Unlink: Removes the link between a story and its author

### Headers 
#### Request 

| Header          | Example value               | Meaning                                                                                                                                                                                                                                                                                                                                                                                                      | 
|-----------------|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| Accept          | Text/html, application/json | The client's preferred format for the response body. Browsers tend to prefer text/html, which is a human-friendly format. Applications using an API are likely to request JSON, which is structured in a machine-parseable way. This can be a list, and if so, the list is parsed in priority order: the first entry is the most desired format, all the way down to the last one.                           | 
| Accept-language | en-US                       | The preferred written language for the response. This is most often used by browsers indicating the language the user has specified as a preference                                                                                                                                                                                                                                                          | 
| User-agent      | Mozilla/5.0                 | This header tells the server what kind of client is making the request. This is an important header because sometimes responses or JavaScript actions are performed differently for different browsers. This is used less frequently for this purpose by API clients, but it's a friendly practice to send a consistent user-agent for the server to use when determining how to send the information back.  | 
| Content-length  | size of the content body    | When sending a PUT or POST, this can be sent so the server can verify that the request body wasn't truncated on the way to the server.                                                                                                                                                                                                                                                                       | 
| Content-type    | application/json            | When a content body is sent, the client can indicate to the server what the format is for that content in order to help the server respond to the request correctly.                                                                                                                                                                                                                                         | 


#### Response 

| Header                       | Example value                       | Meaning                                                                                                                                                                                                                                                                                                                                                                                                      | 
|------------------------------|-------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| Content-Type                 | application/json                    | As with the request, when the content body is sent back to the client, the Content-Type is generally set to help the client know how best to process the request. Note that this is tied somewhat indirectly to the Accept header sent by the client. The server will generally do its best to send the first type of content from the list sent by the client but may not always provide the first choice.  | 
| Access-Control-Allow-Headers | Content-Type, Authorization, Accept | This restricts the headers that a client can use for the request to a particular resource                                                                                                                                                                                                                                                                                                                    | 
| Access-Control-Allow-Methods | GET, PUT, POST, DELETE, OPTIONS     | What HTTP methods are allowed for this resource                                                                                                                                                                                                                                                                                                                                                              | 
| Access-Control-Allow-Origin  | * or http://www.example.com         | This restricts the locations that can refer requests to the resource                                                                                                                                                                                                                                                                                                                                         | 

#### Compression 
* Accept-Encoding/Content-Encoding: 
	- Condition: Content compression occurs only when a client advertises, wants to use it and a server indicates its willingness to enable it. 
		+ Clients indicate they want to use it by sending the Accept-Encoding header when making requests. The value of this header is a comma-separated list of compression methods that the client will accept. For example, Accept-Encoding: gzip, deflate.
		+ If the server supports any of the compression methods that the client has advertised, it may deliver a compressed version of the resource. It indicates that the content has been compressed with the Content-Encoding header in the response. For example, Content-Encoding: gzip. Content-Length header in the response indicates the size of the compressed content. 
	- Methods 
		+ identity: no compression. 
		+ compress: UNIX compress method, which is based on the Lempel-Ziv Welch (LZW) aglorithm
		+ gzip: the most popular format. 
		+ deflate: just gzip without the checksum header. 
	- What to compress:
		+ Usually applied to text-based content such as HTML, XML, CSS, and Javascript.
		+ Not applied to binary data
			* Many of binary formats such as GIF, PNG, and JPEG already use compression. 
	- Disadvantages:
		+ There is additional CPU usage at both the server side and client side. 
		+ There will always be a small percentage of clients that simply can't accept compressed content.

### Parameters 
* Parameters are frequently used in HTTP requests to filter responses or give additional information about the request. They're used most frequently with GET(read) operations to specify exactly what's wanted from the server. Parameters are added to the address. They're separated from the address with a question mark (?), and each key-value pair is separated by an equals sign (=); pairs are separated from each other using the ampersand. 

| Action                                | System call                                                    | HTTP verb address                       | Successful response code / Response body                                                         | 
|---------------------------------------|----------------------------------------------------------------|-----------------------------------------|--------------------------------------------------------------------------------------------------| 
| Get order list, only Trenta iced teas | Retrieve list with a filter                                    | Get /orders?name=iced%20tea&size=trenta | [{ "id" : 1, "name" : "iced tea", "size" : "trenta", "options" : ["extra ice", "unsweetened"] }] | 
| Get options and size for the order    | Retrieve order with a filter specifying which pieces to return | Get /orders/1?fields=options,size       | { "size" : "trenta", "options" : ["extra ice", "unsweetened"]}                                   | 

## HTTP session
### Stateless applications
* Web application servers are generally "stateless":
	- Each HTTP request is independent; server can't tell if 2 requests came from the same browser or user.
	- Web server applications maintain no information in memory from request to request (only information on disk survives from one request to another).
* Statelessness not always convenient for application developers: need to tie together a series of requests from the same user. Since the HTTP protocol is stateless itself, web applications developed techniques to create a concept of a session on top of HTTP so that servers could recognize multiple requests from the same user as parts of a more complex and longer lasting sequence. 


### Structure of a session
* The session is a key-value pair data structure. Think of it as a hashtable where each user gets a hashkey to put their data in. This hashkey would be the “session id”.
 
### Server-side session vs client-side cookie

| Category       | Session                                                                               | Cookie                                            | 
|----------------|---------------------------------------------------------------------------------------|---------------------------------------------------| 
| Location       | User ID on server                                                                     | User ID on web browser                            | 
| Safeness       | Safer because data cannot be viewed or edited by the client                           | A hacker could manipulate cookie data and attack  | 
| Amount of data | Big                                                                                   | Limited                                           | 
| Efficiency     | Save bandwidth by passing only a reference to the session (sessionID) each pageload.  | Must pass all data to the webserver each pageload | 
| Scalability    | Need efforts to scale because requests depend on server state                         | Easier to implement                               | 


#### Store session state in client-side cookies
##### Cookie Def
* Cookies are key/value pairs used by websites to store state informations on the browser. Say you have a website (example.com), when the browser requests a webpage the website can send cookies to store informations on the browser.

##### Cookie typical workflow

```
// Browser request example:

GET /index.html HTTP/1.1
Host: www.example.com

// Example answer from the server:


HTTP/1.1 200 OK
Content-type: text/html
Set-Cookie: foo=10
Set-Cookie: bar=20; Expires=Fri, 30 Sep 2011 11:48:00 GMT
... rest  of the response

// Here two cookies foo=10 and bar=20 are stored on the browser. The second one will expire on 30 September. In each subsequent request the browser will send the cookies back to the server.


GET /spec.html HTTP/1.1
Host: www.example.com
Cookie: foo=10; bar=20
Accept: */*
```

##### Cookie Pros and cons
* Advantage: You do not have to store the sesion state anywhere in your data center. The entire session state is being handed to your web server with every web request, thus making your application stateless in the context of the HTTP session. 
* Disadvantage: Session storage can becomes expensive. Cookies are sent by the browser with every single request, regardless of the type of resource being requested. As a result, all requests within the same cookie domain will have session storage appended as part of the request. 
* Use case: When you can keep your data minimal. If all you need to keep in session scope is userID or some security token, you will benefit from the simplicity and speed of this solution. Unfortunately, if you are not careful, adding more data to the session scope can quickly grow into kilobytes, making web requests much slower, especially on mobile devices. The coxt of cookie-based session storage is also amplified by the fact that encrypting serialized data and then Based64 encoding increases the overall byte count by one third, so that 1KB of session scope data becomes 1.3KB of additional data transferred with each web request and web response. 

#### Store session state in server-side 
* Approaches:
	- Keep state in main memory
	- Store session state in files on disk
	- Store session state in a database
		+ Delegate the session storage to an external data store: Your web application would take the session identifier from the web request and then load session data from an external data store. At the end of the web request life cycle, just before a response is sent back to the user, the application would serialize the session data and save it back in the data store. In this model, the web server does not hold any of the session data between web requests, which makes it stateless in the context of an HTTP session. 
		+ Many data stores are suitable for this use case, for example, Memcached, Redis, DynamoDB, or Cassandra. The only requirement here is to have very low latency on get-by-key and put-by-key operations. It is best if your data store provides automatic scalability, but even if you had to do data partitioning yourself in the application layer, it is not a problem, as sessions can be partitioned by the session ID itself. 

##### Typical server-side session workflow
1. Every time an internet user visits a specific website, a new session ID (a unique number that a web site's server assigns a specific user for the duration of that user's visit) is generated. And an entry is created inside server's session table

| Columns    | Type        | Meaning                       | 
|------------|-------------|-------------------------------| 
| sessionID | string      | a global unique hash value    | 
| userId     | Foreign key | pointing to user table        | 
| expireAt   | timestamp   | when does the session expires | 

2. Server returns the sessionID as a cookie header to client
3. Browser sets its cookie with the sessionID
4. Each time the user sends a request to the server. The cookie for that domain will be automatically attached.
5. The server validates the sessionID inside the request. If it is valid, then the user has logged in before. 

##### Use a load balancer that supports sticky sessions: 
* The load balancer needs to be able to inspect the headers of the request to make sure that requests with the same session cookie always go to the server that initially the cookie.
* But sticky sessions break the fundamental principle of statelessness, and I recommend avoiding them. Once you allow your web servers to be unique, by storing any local state, you lose flexibility. You will not be able to restart, decommission, or safely auto-scale web servers without braking user's session because their session data will be bound to a single physical machine. 

## DNS 
* Resolve domain name to IP address		

### Design
#### Initial design
* A simple design for DNS would have one DNS server that contains all the mappings. But the problems with a centralized design include:
	- **A single point of failure**: If the DNS server crashes, so does the entire Internet. 
	- **Traffic volume**: A single DNS server would have to handle all DNS queries.
	- **Distant centralized database**: A single DNS server cannot be close to all the querying clients. 
	- **Maintenance**: The single DNS server would have to keep records for all Internet hosts. It needed to be updated frequently

#### A distributed, hierarchical database
* **Root DNS servers**: 
* **Top-level domain servers**: Responsible for top level domains such as com, org, net, edu, and gov, and all of the country top-level domains such as uk, fr, ca, and jp. 
* **Authoritative DNS servers**: 
* **Local DNS server**: Each ISP - such as a university, an academic department, an employee's company, or a residential ISP - has a local DNS server. When a host connects to an ISP, the ISP provides the host with the IP addresses of one of its local DNS servers. 

### Internals
****
#### DNS records
* The DNS servers store source records (RRs). A resource record is a four-tuple that contains the following fields: (Name, Value, Type, TTL )
* There are the following four types of records
	- If Type=A, then Name is a hostname and Value is the IP address for the hostname. Thus, a Type A record provides the standard hostname-to-IP address mapping. For example, (relay1.bar.foo.com, 145.37.93.126, A) is a Type A record
	- If Type=NS, then Name is a domain and Value is the hostname of an authoritative DNS server that knows how to obtain the IP addresses for hosts in the domain. This record is used to route DNS queries further along in the query chain. As an example, (foo.com, dns.foo.com, NS) is a Type NS record. 
	- If Type=CNAME, then Value is a canonical hostname for the alias hostname Name.
	- If Type=MX, then Value is the canonical name of a mail server that has an alias hostname Name. 

#### Insert records into DNS DB 
* Take domain name networkutopia.com as an example.
* First you need to register the domain name network. A registrar is a commercial entity that verifies the uniqueness of the domain name, enters the domain name into the DNS database and collects a small fee for its services.
	- When you register, you need to provide the registrar with the names and IP addresses of your primary and secondary authoritative DNS servers. For each of these two authoritative DNS servers, the registrar would then make sure that a Type NS and a Type A record are entered into the TLD com servers.

	
#### DNS query parsing
* When a user enters a URL into the browser's address bar, the first step is for the browser to resolve the hostname (http://www.amazon.com/index.html) to an IP address. The browser extracts the host name www.amazon.com from the URL and delegates the resolving task to the operating system. At this stage, the operating system has a couple of choices. 
* It can either resolve the address using a static hosts file (such as /etc/hosts on Linux) 
* It then query a local DNS server.
	- The local DNS server forwards to a root DNS server. The root DNS server takes not of the com suffix and returns a list of IP addresss for TLD servers responsible for com domain
	- The local DNS server then resends the query to one of the TLD servers. The TLD server takes note of www.amazon. suffix and respond with the IP address of the authoritative DNS server for amazon. 
	- Finally, the local DNS server resends the query message directly to authoritative DNS which responds with the IP address of www.amazon.com. 
* Once the browser receives the IP addresses from DNS, it can initiate a TCP connection to the HTTP server process located at port 80 at that IP address. 

### Types
#### Round-robin DNS
* A DNS server feature that allowing you to resolve a single domain name to one of many IP addresses. 

#### GeoDNS
* A DNS service that allows domain names to be resolved to IP addresses based on the location of the customer. A client connecting from Europe may get a different IP address than the client connecting from Australia. The goal is to direct the customer to the closest data center to minimize network latency. 

### Functionality
#### DNS Caching 
* Types:
	- Whenever the client issues a request to an ISP's resolver, the resolver caches the response for a short period (TTL, set by the authoritative name server), and subsequent queries for this hostname can be answered directly from the cache. 
	- All major browsers also implement their own DNS cache, which removes the need for the browser to ask the operating system to resolve. Because this isn't particularly faster than quuerying the operating system's cache, the primary motivation here is better control over what is cached and for how long.
* Performance:
    - DNS look-up times can vary dramatically - anything from a few milliseconds to perhaps one-half a second if a remote name server must be queried. This manifests itself mostly as a slight delay when the user first loads the site. On subsequent views, the DNS query is answered from a cache. 

#### Load balancing
* DNS can be used to perform load distribution among replicated servers, such as replicated web servers. For replicated web servers, a set of IP addresses is thus associated with one canonical hostname. The DNS database contains this set of IP addresses. When clients make a DNS query for a name mapped to a set of addresses, the server responds with the entire set of IP addresses, but rotates the ordering of the addresses within each reply. Because a client typically sends its HTTP request to the IP address that is the first in the set, DNS rotation distributes the traffic among the replicated servers. 

#### Host alias
* A host with a complicated hostname can have one or more alias names. For example, a hostname such as relay1.west-coast.enterprise.com could have two aliases such as enterprise.com and www.enterprise. 

### DNS prefetching
#### Def
* Performing DNS lookups on URLs linked to in the HTML document, in anticipation that the user may eventually click one of these links. Typically, a single UDP packet can carry the question, and a second UDP packet can carry the answer. 

#### Control prefetching
* Most browsers support a link tag with the nonstandard rel="dns-prefetch" attribute. This causes teh browser to prefetch the given hostname and can be used to precache such redirect linnks. For example

> <link rel="dns-prefetch" href="http://www.example.com" >

* In addition, site owners can disable or enable prefetching through the use of a special HTTP header like:

> X-DNS-Prefetch-Control: off

## Load balancers 
### Benefits 
* Decoupling
	- Hidden server maintenance. You can take a web server out of the load balancer pool, wait for all active connections to drain, and then safely shutdown the web server without affecting even a single client. You can use this method to perform rolling updates and deploy new software across the cluster without any downtime. 
	- Seamlessly increase capacity. You can add more web servers at any time without your client ever realizing it. As soon as you add a new server, it can start receiving connections. 
	- Automated scaling. If you are on cloud-based hosting with the ability to configure auto-scaling (like Amazon, Open Stack, or Rackspace), you can add and remove web servers throughout the day to best adapt to the traffic. 
* Security
	- SSL termination: By making load balancer the termination point, the load balancers can inspect the contents of the HTTPS packets. This allows enhanced firewalling and means that you can balance requests based on teh contents of the packets. 
	- Filter out unwanted requests or limit them to authenticated users only because all requests to back-end servers must first go past the balancer. 
	- Protect against SYN floods (DoS attacks) because they pass traffic only on to a back-end server after a full TCP connection has been set up with the client. 

### Round-robin algorithm 
* Def: Cycles through a list of servers and sends each new request to the next server. When it reaches the end of the list, it starts over at the beginning. 
* Problems: 
	- Not all requests have an equal performance cost on the server. But a request for a static resource will be several orders of magnitude less resource-intensive than a requst for a dynamic resource. 
	- Not all servers have identical processing power. Need to query back-end server to discover memory and CPU usage, server load, and perhaps even network latency. 
	- How to support sticky sessions: Hashing based on network address might help but is not a reliable option. Or the load balancer could maintain a lookup table mapping session ID to server. 


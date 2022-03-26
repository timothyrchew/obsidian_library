Specifics to practice:
- back of envelop calculations on the fly


# Structured approach

- Ask clarifying questions
- Lay out functional reqs
	- what functionality should the system support
- Lay out non-functional reqs
	- what are the key characteristics of the system
- Determine capacity estimates

# Non functional reqs

Scalable
Highly available
Low latency
Durable
Write heavy vs Read heavy


# Capacity Estimates

great overview of pieces to remember for BOE calcs: https://itsallbinary.com/system-design-back-of-envelop-calculations-for-storage-size-bandwidth-traffic-etc-estimates/

Determine the volume of what we are serving up
- determine users
- determine how many distinct actions users will be conducting
- calculate capacity
	- queries per second
	- bandwidth per second
	- storaget reqs over 5 years


key things to remember:
- 100k seconds in a day
- 60 months in a 5 years

capacity estimates:
- megabyte is 1 million (10^6)
- gigabyte is 1 billion (10^9) (1 mill KB is 1 GB)
- terbyte is 1 trillion (10^12)

50GB *

things to remember
- determine how many actions in a month
- multiply that by 60 to get 5 year storage
- divide monthly actions by 30 to get to queries per day. then divide by 100k (seconds per day)


# Distributed Systmes

Cap Theorem
- impossible for a system to provde all three
	- Consistency
	- Availability
	- Partition Tolerance
- realistically you can't lose data, so you either pick between availability or consistency

# Caching

cache invalidation
- need to keep the cache coherant with the database. if the DB is modified, it should be invalidated in the cache otherwise we get  inconsitent behavior
- three approaches
	- write-through cache 
		- when data is written, write it to the cache as well. downside is that this adds some latency for write operations
	- write around cache
		- data is written directly to permanent storage, bypassing the cache. disadvantage is that fetching recently written data will create a cache miss and must be read from slower backend storage with higher latency
	- write back cache
		- data is written to cache alone, and then it is copied to permanent storage after a specified interval. increased risk of losing data in this case.

cache eviction
- LRU (least recently used)

# Data partitioning

Sharding
- Horizontal partioning (divide rows into different partitions). each table has the same schema and col, but entirely different rows. data in each table is unqiue.

Vertical scaling is splitting different columns into their own tables


partitioning criteria
- key or hash based partitioning
	- apply hash funtion to some key attribute of the entitiy and that yeilds the partiion number. downside is that this fixes the total number of Db servers, since adding new servers means chaning the hash function.
- list partitioning
	- each partition is assigned a list of values. whenever we go to insert a value we see which partition constains the range for our inerstion. example would be to do this based on country of origin. 
- round robin


# Indexes

when DB performance is no longer satisfactory, indexing is one of the first things you can turn to in order to get performance boosts. indexes would be performned on a specific col to increase the speed at which that key can be looked up. the largest benefit is improving the performance is search queries. 

indexing can significantly speed up data retreival, but at the expense of write performance. when writing to a table, the write action has to also update the index. this applies to all insert, update, delete operations.

adding unnecessary indexes should be avoided and indexes that are no longer used should be removed.

# proxy

forward proxy (or just proxy) is generally intended to protect the client. it can route all requests from a local network through a proxy and can act as the single gateway. it can enforce security protocols, convert and mask IP, and block unknown traffic.

a reverse proxy is used to protect servers. it accepts requests from a client, and then forwards the request to one of many other servers, finally returning the request back to the client as if the proxy had processed the request itself. the client just communicates directly with the reverse proxy.


typicall used to filer requests, log requests, or sometimes transform requests (adding / removing headers, encrypting / decrypting, or compressing resources). another benefit is that if multiple clients acess a particular resource, the proxy server can cache it and serve it to all the clients without going to the remote server

reverse proxy vs load balancer
- i have 12 servers each of which is capable of serving a session to a user. i use a load balancer to assign individual sessions to each service based on their use
- i have 12 servers providing different services (db, web content, appliation). my reverse proxy routes the incoming traffic based on what it is requesting.


# Redundancy and Replication

Redundancy is duplication of critical components with the intention of increasing the reliability of the system, usually in the form of a backup or fail safe. redunancy is aimed a removing the single points of failure in the system and provides backups if needed in crisis. 

Replication means sharing info to ensure consistency between redundant resources. replication is widely used in many database management systems, usually with a primary-replica relationship between the originals and the copies. 

# Load balancers

- distributes traffic across a cluster of servers
- if a server goes down or is not available, the LB can redirect traffic to another server
- it can help prevent any one application server from becoming a single point of failure

connection methods
- least connetion method
- least response time method
- least bandwidth method 
- round robin method








# DB Design


## NoSQL

examples
- Redis (key-value)
- MongoDB (docuent style)
- Cassandra (columnar style)

most nosql databases sacrifice ACID compliance for better performance and scalability

when to use:
- storing large volumes of data that little to no structure
- making the most of cloud storage. NoSQL databases are designed to be scalled across multipel data centers out of the box.
- good for rapid develoment as it doesn't need to be prepped ahead of time. good for quick iterations.



## MySQL

much more powerful queries possible, compared to basically just looking up a collection of documents

harder to scale horizontally, easier to scale vertically. can be scaled horizontally, but it is "challenging and timeconsuming"

Most SQL databases will give us ACID compliance. 

when to use:
- need ACID compliance
- data is structured and unchanging
- need complex queries / relational lookups


# Scaling

Cassandra and MongoDB are good for horizontal scaling- adding more machines to meet growing needs

MySQL is better at vertical scaling. 


# Communications Protocols

http
- client opens up a connection and requests data from the server
- the server calculates the response
- the server sends the response back to the client on the opened request

ajax polling
- client repeatedly requests data from the server using http
- if there is new data, the server responds. if not it sends empty requsts
- client repeatedly pings, causing lots of http overhead

http long polling (or "hanging get")
- if the server does not have any data available for the client, instead of sending an empty response, the server holds the request and waits until some data becomes available
- once data is available, a full response is sent back to the client. the client then immediately re-requests info from the server so that the server will almost always have an available waiting request that it can use to deliver data in response to an event

web sockets
- provides a persistent connection between a client and a server over a single TCP connection. The websocket is established through a websocket handshake. if it succeeds, both the server and client can exchange data in both directions at any time. two-way, bi-directional ongoing conversation can take place.

server-sent events
- client establishes a persistent long term connection with the server. the server uses this to send data to the client, however its one way. if the client wants to send to the server, it would require the use of another protocol.
- SSE is best when we need real-time traffic from the server to the client.


# CDN

solution for minimizing request latency when fetching static assets from a server. an ideal CDN is composed of a group of servers spread out globally, such that no matter how far a user is from my server, they will always be close to a CDN server. Then instead of having to fetch static assets (images, video, html/css/js) from the origin server, user can fetch cached copies of these files from the CDN more quickly

because static assets can be very large, we are also saving our network a lot of bandwidth

two primary ways for CDN cache to be populated
- push CDN
	- origin server sends assets to the CDN, which stores it in its cache. the CDN never makes any requests to the origin server
- pull CDN
	- if the CDN doesn't have the static asset in its cache, then it forwards the request to the origin server and then caches the new assets.


CDNs are good for reducing request latency and network bandwidth, but here are some cases where they woudl not be a good idea
- all users are in a specific region. better to just host origin server there.
- not good for dynamic content or sensitive content. you risk serving up stale data (bad idea if working in financial or govt services)
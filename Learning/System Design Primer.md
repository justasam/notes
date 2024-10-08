
## Scalability

##### Vertical scaling
Upgrading the *same* machine to meet the demand (e.g. better/more CPUs, RAM, storage).
Has a hard limit (state of the art in technology).
##### Horizontal scaling
*Multiple* interconnected machines. 
The traffic needs to be distributed through the servers, a task for which a *load balancer* is typically used.  The exposed (public) IP is then usually one of the load balancer, but the machines can communicate with each other with private IPs.
##### Load balancer
A load balancer is a networking device or software (e.g. in cloud environments) that distributes incoming network traffic across multiple servers or resources to optimize performance and ensure high availability.
The simplest form of a load balancer is a *DNS server*: if multiple IP addresses are configured for the same hostname in DNS records, it typically resolves the IPs for a request in a round-robin fashion. It does not take into account the availability of individual servers and DNS server response is typically cached (so server 1 could be the only server a user interacts with until the cache expires).
Things like server load could be taken into account to make a better load balancer.

> [!info]- What is a DNS server?
> A DNS server is like a phonebook for the internet, translating domain names (like google.com) into IP addresses (like 172.217.12.14) so that your computer can connect to websites and services.
##### Shared session state
If the sessions are kept on the servers, load balancing could result in them getting lost. For example, a user's login session may be stored on server A, but the load balancer returns server B (which does not have the user's session) when making another request.
The solution for this issue is to use a dedicated server for session state.
From that, another problem arises: if session state server goes down, then all the other servers are rendered (mostly) useless.  Technologies like RAID could help alleviate that.

> [!info]- What is RAID?
> **RAID** (**R**edundant **A**rray of **I**ndependent **D**isks) is a technology used to combine multiple hard drives into a single unit for improved performance, redundancy, or both.
> **Raid 0** introduces *striping* -- a technique where data is split into blocks and spread across multiple disks in a stripe-like fashion. This aims to improve performance, by allowing data to be accessed in parallel from multiple disks.
> **RAID 1** focuses on *redundancy* instead of performance: data is mirrored across two or more disks. If one disk fails, the data remains accessible from the remaining disks. 
> **RAID 5** is a variant of RAID 1: Only one of X drives (parity of 1), is used for redundancy.
> **RAID 6** is a variant of RAID 1: Two of X drives (parity of 2) are used for redundancy.
> **RAID 10** (also known as RAID 1+0)  combines features of both RAID 1 and RAID 0. The data is first mirrored onto separate pairs of disks, and then the mirrored sets are striped together to provide redundancy and performance benefits. This requires a minimum of four disks.
##### Sticky sessions (Session affinity)
Sticky sessions are an alternative to shared session state, where subsequent requests from the same client are directed to the same server or instance that served the initial request. They often work by storing a session cookie with a unique ID, allowing the load balancer to determine which server handled the initial request.
##### Caching
Caching is a technique used to store copies of frequently accessed data or resources in a temporary location, such as memory or disk, to speed up access and reduce the need to repeatedly fetch the data from its original source. Caches are finite, so garbage collection is typically implemented to manage them. This is often done by setting expiry times, allowing data that hasn't been accessed for a while to be deleted.
##### Database replication
Database replication is the process of copying and maintaining the same data across multiple databases or servers in real-time or near real-time to ensure data consistency, availability, and fault tolerance. 
Typically, there is a master database, with slaves that replicate queries from the master. Slaves can be utilized for redundancy or to balance read requests, as many websites are read-heavy. Most of the slaves handle read requests, while only a few are dedicated to writes, which are eventually propagated to all databases.
Multiple master databases could also be used for fault tolerance and higher availability, as well as geographic distribution. It's important to keep in mind, that there are challenges with managing that, and robust replication mechanism is essential to ensure data integrity and reliability.
##### Database partitioning
Database partitioning is the process of dividing a large database into smaller, more manageable segments called partitions. Each partition contains a subset of the data and can be managed and accessed independently. This helps improve performance, scalability, and maintenance of the database.
Combining database partitioning with replication allows for efficient distribution of data and workload across multiple servers or nodes, enhancing performance, scalability, and fault tolerance in distributed database systems.

**Source**: [Scalability Lecture at Harvard](https://www.youtube.com/watch?v=-W9F__D3oY4)

---
## Scalability II

##### Clones
Every server should contain exactly the same codebase and should not store any user-related data, such as sessions or profile pictures, on local disk or memory. Sessions need to be stored in a centralized data store accessible to all servers, such as an external database or persistent cache like Redis. Image files can be created from the servers (AWS calls this AMI - Amazon Machine Image). AMIs can then be used as a "super-clone" from which all new instances are based upon.

##### Database
Eventually, even a horizontally scaled application will become slower and slower until it finally breaks down. The main reason for this is typically the database, such as MySQL. To address this issue, there are two paths:
1) Stick with MySQL, implement master-slave replication, upgrade servers, eventually shard the database, and denormalize it. However, at this point, every action will become more expensive and time-consuming than the previous one.
2) Start denormalizing from the beginning and avoid using joins in any database query. If sticking with MySQL, consider using it as a NoSQL database, or switch to MongoDB, etc. Joins will now be performed in the application code. Even after making these changes and moving joins to the application, requests will slow down when a lot of data is fetched, necessitating the introduction of a cache.
##### Cache
A cache is a simple key-value store and it should reside as a buffering layer between application and data storage. Datasets in cache are typically held in RAM and requests are handled as fast as technically possible.
There are 2 patterns of caching data, an old one and a new (preferred) one:
###### 1 - Cached Database Queries
Whenever a query is made to the database, the result dataset is stored in cache. A hashed version of query is the cache key. The next time you run the query, you first check if it's already in cache.
The main issue of this pattern is expiration: it is hard to delete a cached result, when a complex query is cached. When one piece of data changes, you need to delete all queries which might include that piece of data.
###### 2 - Cached Objects
This refers to storing "assembled" data arrays from instances, or better yet the complete instances themselves, in the cache. It allows to easily get rid of the object whenever something changes, and makes the overall operation of code faster and more logical.

Some things that are typically cached:
- User sessions
- Fully rendered blog articles & other SSR pages
- activity streams
- user<->friend relationships

##### Asynchronism
There are typically two paradigms for handling asynchronism:
###### Async #1
This method involves pre-processing time-consuming tasks in advance, allowing for the serving of finished work with minimal request time. It's commonly employed to convert dynamic content into static content, such as generating pages for a website. These tasks are often scheduled to run periodically, for example, through a script executed by a cronjob every hour.
###### Async #2
In cases where pre-processing isn't feasible due to highly dynamic requirements, particularly for computationally intensive tasks, a job queue is utilized. This approach ensures that users are immediately notified and can continue using the page without being blocked. Workers continuously monitor the queue, picking up jobs as they become available, and signal completion once tasks are finished. This signal is then relayed through the front-end to the user.
Systems like RabbitMQ facilitate the implementation of asynchronous processing, although even a simple Redis list could suffice: tasks can be pushed onto the list and popped by workers monitoring the queue.

In situations where time-consuming tasks need to be performed, asynchronous execution is recommended.

**Source**: [Scalability, Le Cloud Blog](https://web.archive.org/web/20221030091841/http://www.lecloud.net/tagged/scalability/chrono)

---
## Performance vs scalability

A service is **scalable** if it results in increased **performance** proportional to resources added. 
- If you have a **performance** problem, your system is slow for a single user.
- If you have a **scalability** problem, your system is fast for a single user but slow under heavy load.
In distributed system there are other reasons for adding resources to the system; for example to improve the reliability. *An always-on service is said to be scalable if adding resources to facilitate redundancy does not result in a loss of performance*.
Scalability can't be an afterthought because retrofitting scalability into a system that wasn't designed for it can be exceedingly difficult and costly, for example: algorithms which might work for smaller datasets can be exceedingly slow on larger ones, thus forcing to find a new solution which can be difficult and time-consuming.

**Source**: [A word on scalability, All things distributed](https://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html)


	
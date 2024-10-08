The Domain Name System (DNS) is the phonebook of the Internet. Humans access information online through domain names. Web browsers interact through IP addresses. DNS translates domain names to IP addresses so browsers can load Internet resources.
Each device connected to the Internet has a unique IP address.

#### How does DNS work?
DNS resolution involves converting a hostname (such as www.example.com) into a computer-friendly IP address (such as 192.168.1.1).
There are 4 DNS servers involved in loading a webpage:
- **DNS recursor** - it can be thought of as a librarian who is asked to go find a particular book somewhere in a library. The DNS recursor is a server designed to receive queries from client machines through applications such as web browsers. Typically the recursor is then responsible for making additional requests in order to satisfy the client's DNS query.
- **Root nameserver** - the root server is the first step in resolving human readable host names into IP addresses. It can be thought of like an index in a library that points to different racks of books - typically it servers as a reference to other more specific locations.
- **TLD nameserver** - the top level domain server can be thought of as a specific rack of books in a library. It hosts the last portion of a hostname (In example.com, the TLD server is "com").
- **Authoritative nameserver** - this final nameserver can be thought of as a dictionary on a rack of books, in which a specific name can be translated into its definition. It is the last stop in the nameserver query. If it has access to the requested record, it will return the IP address for the requested hostname back to the DNS recursor that made the initial request.

![[Pasted image 20241006180421.png]]

#### The 8 steps in a DNS lookup
When DNS information is cached, steps are skipped from DNS lookup process which makes it quicker. Steps below assume nothing is cached.
1. User types 'example.com' into a web browser and the query travels into the Internet and is received by a DNS recursive resolver.
2. The resolver then queries a DNS root nameserver (`.`).
3. The root server then responds to the resolver with the address of a TLD DNS server (such as .com or .net), which stores the information for its domains. When searching for example.com, our request is pointed toward the .com TLD.
4. The resolver then makes a request to the .com TLD.
5. The TLD server then responds with the IP address of the domain's nameserver, example.com.
6. Lastly, the recursive resolver sends a query to the domain's nameserver.
7. The IP address for example.com is then returned to the resolver from the nameserver.
8. The DNS resolver then responds to the web browser with the IP address of the domain requested initially.

#### What are the types of DNS queries?
In a typical DNS lookup three types of queries occur. By using a combination of these queries, an optimized process for DNS resolution can result in a reduction of distance traveled. In an ideal situation cached record data will be available, allowing a DNS server to return a non-recursive query.
##### 3 types of DNS queries:
1. **Recursive query** - in a recursive query, a DNS client requires that a DNS server (typically a DNS recursive resolver) will respond to the client with requested resource record or an error message.
2. **Iterative query** - in this situation the DNS client will allow a DNS server to return the best answer it can. If it does not have a match for the query name, it will return a referral to a DNS server authoritative for a lower level of the domain namespace. The DNS client the will make a query to the referral address. This process continues with additional DNS servers down the query chain until either an error or timeout occurs.
3. **Non-recursive query** - typically this will occur when a DNS resolver client queries a DNS server for a record that it has access to either because it's authoritative for the record or the record exists inside of its cache. Typically, a DNS server will cache DNS records to prevent additional bandwidth consumption and load on upstream servers.

#### What is DNS caching? Where does DNS caching occur?
DNS caching involves storing data closer to the requesting client so that the DNS query can be resolved earlier and additional queries can be avoided, thereby improving load times and reducing bandwidth/CPU consumption. It can be cached in various locations, each of which will store DNS records for a set amount of time determined by a time-to-live (TTL).

##### Browser DNS caching
Modern web browsers are designed by default to cache DNS records for a set amount of time. When a request is made for a DNS record, the browser cache is the first location checked for the requested record.

##### Operating system (OS) level DNS caching
The OS level DNS resolver is the second and the last local stop before a DNS query leaves your machine. The process inside the OS that is designed to handle this query is commonly called a "stub resolver" or DNS client. When it gets a request from an application, it first checks its own cache (or the hosts file), and if does not have the record it then sends a DNS query (with a recursive flag set), outside the local network to a DNS recursive resolver inside the ISP.

Source: *[Cloudflare](https://www.cloudflare.com/en-gb/learning/dns/what-is-dns/)*

---

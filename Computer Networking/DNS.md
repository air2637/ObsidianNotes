## Definition

[What is DNS, and how DNS works?](https://www.cloudflare.com/en-gb/learning/dns/what-is-dns/)

Domain Name System - a phone book of the internet, that translates domain names e.g. nytimes.com to IP addresses.

1. DNS recursor: a librarian who is asked to find a particular book somewhere in a library 
2. Root nameserver: an index in a library that points to different racks of books, so it is a reference to other more specific locations
3. TLD (Top Level Domain) server: a specific rack of books in a library, it hosts the last portion of a hostname, e.g '.com', '.net'
4. Authoritative nameserver: a dictionary on a rack of books, in which a specific name can be translated into its definition. so it returns the IP address
![[Screenshot 2023-02-12 at 2.32.43 PM.png]]
## DNS Caching 

> DNS caching involves storing data closer to the requesting client so that the DNS query can be resolved earlier and additional queries further down the DNS lookup can be **avoided** -> improves load times and reduce cpu consumptions.

### Various candidate caching locations:

1. Browser DNS caching, e.g. Chrome store the caches at chrome://net-internals/#dns
2. OS level DNS caching
	- last local stop before DNS query leaves your machine
	- "Stub resolver"/"DNS client" - the process in OS handle the DNS query
	- if the stub resolver fails to find the domain locally, query ==DNS recursive resolver in side the Internet Service Provider (ISP)==



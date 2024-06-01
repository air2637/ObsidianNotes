# Preface

- What is considered as a *data intensive application*?

If data is the primary challenge, in terms of the *quantity, complexity, or the speed at which it is changing* - and opposed to *compute intensive application* where cpu cycles are the bottleneck. 

- What is this book focused on?

There are numbers of buzzwords in area of data intensive applications, e.g. *NoSQL, Big data, web scale, sharding, Eventual consistency, ACID, Cap theorem, Cloud services, Map Reduce etc* , but this book focuses on the technically accurate and precise understanding of various technologies, and knowing their trade-offs. **Behind rapid changing technologies, the enduring principles that remains true.** By having a good understanding of the principles, we should be in good position to see where each tool fits in, how to make good use of it, and how to avoid its pitfalls. 

- Breakdown of the chapters in the book?

It contains:
1. fundamentals that applicable to all systems:
	1. 3 goals - reliability, scalability, and maintainability, how are they defined, and how are they achieved
	2. distinguishing data models and query languages
	3. distinguishing storage engines - how DB arrange data on a disk, so data can be retrieved efficiently
	4. distinguishing formats for data encoding / serialization , and the evolution of schemas
2. data stored in one node vs distributed across multiple nodes , and the challenges
	6. partitioning/sharding - chapter 6
	7. transactions - chapter 7
	8. problems in distributed systems - chapter 8
	9. consistency and consensus in a distributed system - chapter 9
3. Derive datasets from heterogeneous systems:
	1. batch processing for data retrieval - chapter 10 
	2. stream processing for building - chapter 11
	3. Future trends - chapter 12

	

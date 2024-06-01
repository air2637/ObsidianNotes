
# Explanation in short

- Reliability: tolerating hardware, software faults and human errors
- Scalability: measuring load & performance, e.g. latency percentiles and throughput. System should be ready to handle high volume, more complex data, and network volume
- Maintainability: Operability, simplicity and evolvability - as eventually not only software developer, but also devops, techops take participant into the run of the solution

# Typical features of a data intensive application

- Store and retrieve data - database
- Remember and compute result of an expensive operation - *cache*
- Search data by keyword - *search indexes*
- Send a message to another process and data is to be handled asynchronously - *stream processing*
- Periodically crunch a large amount of accumulated data - *batch processing*

data systems of small component & made up of component:
- datastores for message queues - *redis*
- message queues with database-like durability guarantees - apache kafka
- application's responsibility that keeps caches (e.g. *Memcache*) or indexes (e.g. *ElasticSearch*) in sync with the database

When your system solution is actually made up by various small components besides you main application logic - you are actually a data system designer! Along the solution, you need to face the challenges in aspects of reliability, scalability and maintainability!

# Reliability

## Fault vs Failure

Fault refers to a component of the system is failed functioning, while failure refers to the entire system fails to serve. Quote from the book: *A fault is usually defined as one component of the system deviating from its spec, whereas a failure is when the system as a whole stops providing the required service to the user.*
## Prefer tolerating faults over preventing faults
 
Tolerating faults when there is no cure existing. e.g. Wrong user inputs, that can be tolerated by system, but can't be prevented.
## hardware redundancy plus software fault-tolerance techniques

Seeing a short downtime for most applications, during which a patch can be quickly done. Introducing redundancy of hardware components can eliminate the downtime.
Systems having software fault-tolerance in addition to redundant hardware components can eliminate the downtime of the system, e.g. *rolling upgrade* - patch one node at a time, while the other node continue serving.

## software errors

Besides commonly seen software exceptions, pay attention to **systematic error** that one error happened causes correlated failures across multiple components (even on different nodes). e.g. Cascading failures. 

There is no quick solution to this. But practices:
- thorough testing
- allow individual component / entire system as a whole to restart
- provide component level guarantee e.g. the number of inputs taken in by a component matches the number of output produced regardless of success or failure in running
- component can raise alert if error found

## Human errors 

Unreliable human, their misconfigurations to system might lead more error than hardware faults. How to make system still reliable in spite of unreliable humans?

- Only expose what user needs, and this help minimises the opportunities for human input errors
- Test thoroughly at all levels, including unit tests, integration tests and manual tests
- Allow quick and easy recovery methods, e.g. fast rollback by configuration changes, rollout new feature gradually so that only small set of users are impacted, *provide tools to recompute data*
- Set up detailed and clear monitoring, e.g. performance metrics, error rates. Show early warning signals.
- Of course, good management practices, trainings


# Scalability

Even if a system is working reliably today, that doesn't mean it will necessarily work reliably in the future. Cases like increased load can adversely impact the system running. 

> Scalability is the term we use to describe a system's ability to cope with increased load.

## Describe load

Only we can succinctly describe the current load on the system, then we can discuss the growth questions. e.g. 

For Tweeter posts, a user can publish a new message to their followers *4.6 requests / sec on average, over 12k requests / sec at peak.* While A user reads their home page timeline for posts by their followees is at average 300k requests / sec. That means most server requests are of read type instead of write type. 

Now 2 approaches in handling above function:
1. Inserting one copy of the post message into `tweets` table, and joining other tables for read requests:

```sql
sel tweets.*, users.* 
from tweets
join users on tweets.sender_id = user.id 
join follows on follows.followee_id = user.id // find folllowers of current posted tweets writer
where follows.follower_id = current_user
;
```

2. Duplicate with cache - that a cache here is like a mailbox for each followers, and when a followee posts a tweet, lookup all followers, and insert the new tweet to their cache. 

Approach 2 is more preferred if the number of followers is not extremely large, as we have much more read requests than write requests. Hence, approach 1 that maintains a single copy of post, and response each request request with joining operations is too much to handle.

Actual solution adopted, is a mix of both approaches - adopting approach 1 if the number of followers is big, while adopting approach 2 if the number is small. 

## Describe Performance

With knowing the load being measured, we can now access the performance by varying the load parameter while **keeping the resources unchanged (CPU, memory, network bandwidth etc).** Alternatively, with increased load parameter, **in order to keep the performance unchanged, how much more resources is required.**

Performance could refer to a system **throughput** e.g. number of requests handled within a second; could refer to **response time**. Median value could probably better describe the performance than Mean value, as it shows how many users actually experiencing the delay. e.g Median response time of 200 ms shows that half of the users get response in less than 200 ms while the other half gets longer than that. 

Combining the mean the median, we can understand the skewness. Left skewed if the mean less than median. 

In queuing delay, we need to help reduce the **head of line blocking** where a small number of slow requests to hold up the processing of subsequent requests. 

## Cope with load

An architecture that is appropriate for one level of load is unlike to cope with 10 times that load. So for a fast growing service, it is advisable to **rethink the architecture on every order of magnitude load increase**!

*Scaling up: aka vertical scaling*, by moving to a more powerful machine.
*Scaling out: aka horizontal scaling*, by distributing the load aross multiple smaller machines. 

*Elastic system* that can keep detecting the load changes, and adjust more / less resources according to the load. Elastic system can be useful if load is highly unpredicable.

System design varies against its volume, and most of the time is very specific. e.g. a system designed to handle 100k requests/sec, and each request take 1kb in memory size, looks very different from a system handling 3 requests/sec, and each request takes 2GB in size. e.g. Twitter's hybrid mode is an example. 

# Maintainability 

It includes investigating failures, adapting it to new platforms, modifying it for new use cases, adding new features, repaying technical debts etc. So when designing the software, we should keep it in mind that the software should hopefully minimise the pain during its maintenance, and avoid creating legacy software ourselves. 

- Make it easy for operation teams
	- monitor the health of the system and allow quick restoring of service from bad state
	- allow easy tracking down the root cause of problems
	- keep software, security patches, platform up to date
	- good documentation
	- *ZW's own idea - the log should document down clearly the general steps/ critical milestones during the data processing*
- Simple for engineers to understand the system
	- avoid explosion of the state space, tight coupling of modules (*so on the opposite, the module boundary is clearly defined*), tangled dependencies, inconsistent naming and terminology, hacks aimed at solving urgent issues etc 
- Easy for engineer to make changes to the system in the future
	- Agile - TDD development pattern, but how to adopt the agile approach for large scaled system? e.g. in the twitter example, moving the implementation from approach 1 to 2?


Later in the book, we will look at certain patterns, techniques that helps achieving the goal of building reliable, scalable and maintainable system.








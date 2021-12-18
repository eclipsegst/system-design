# Cache

The cache tier is a temporary data store layer for those benefits:[1]

- Better system performance by accessing data faster without, e.g. fetching from database or any unnecessary computing
- Reduce database workload

Here's a typical request flow in a system with a cache tier,

- A web server receives a request from a client.
- The web server first checks if the cache has the available response.
- If the cache has the data, send it back to client.
- If not, the web server will queries the database, store the response in cache, and send it back to the client.

## Consideration when using cache

- **Decide when to use cache.** Use when data is read frequently but modified infrequently. A cache server will lose all the data when restart.([Redis supports "snapshots"](https://redis.io/topics/persistence))
- **Expiration policy.** It's good to set expiration policy. Once cached data is expired, it will be removed from the cache. Too short expiration policy will cause the system reload data more frequently, if it's too long, the data might be stale.
- **Consistency.** Keep the data sture and the cache in sync. In consistency can happen because data-modifying operations on the data store and cache are not in a single transaction. Maintaining consistency between data store and cahce across region is challenging.
- **Mitigating failures.** A single cache server represents a potential single point of failure(SPOF).
  - Multiple cache servers across different data centers are recommended to avoid SPOF.
  - Another recommended approach is to overprovision the required memory by certain percentages. This provides a buffer as the memory usage increases.
- **Eviction Policy.** Once the cache is full, any requests to add items to the cahce might cause existing items to be removed.

## Redis

> Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache, and message broker.

### Popular Redis Use Cases

- [Caching](https://redis.io/topics/lru-cache): Redis is a great choice for implementing a highly available in-memory cache to decrease data access latency, increase throughput, and ease the load off your relational or NoSQL database and application. [3]
- [Chat, messaging, and queues](https://redis.io/topics/pubsub): Redis supports Pub/Sub with pattern matching and a variety of data structures such as lists, sorted sets, and hashes. This allows Redis to support high performance chat rooms, real-time comment streams, social media feeds and server intercommunication.[3]
  - [Redis Best Practices: Pub/Sub](https://redis.com/redis-best-practices/communication-patterns/pub-sub/)

### Redis Cluster

>Redis Cluster provides a way to run a Redis installation where data is automatically sharded across multiple Redis nodes.

Redis Cluster means[3],

- The ability to automatically split your dataset among multiple nodes.
- The ability to continue operations when a subset of the nodes are experiencing failures or are unable to communicate with the rest of the cluster.

### [Memcached vs Redis](https://stackoverflow.com/questions/10558465/memcached-vs-redis)

## Reference

- [1] [System Design Interview - An Insider's Guide], Chapter 1 Scale From Zero to Millions of Users, Cache
- [2] [Redis](https://redis.io/)
- [3] [What is Redis? by Amazon](https://aws.amazon.com/redis/)
- [4] [Redis cluster tutorial](https://redis.io/topics/cluster-tutorial)
  - [Redis Cluster Specification](https://redis.io/topics/cluster-spec)

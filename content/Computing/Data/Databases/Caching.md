---
aliases:
  - cache
  - caching
  - Cache
---

Keeps recently used data in fast access memory so that it can be accessed in short time 
Accessing data from a disk takes ~ 1ms
Accessing data from memory ([[RAM]]) takes 100ns (roughly 100x faster)

# Where to Cache
## 1. External Cache
- Redis/Memcached. 
- Stored on a dedicated RAM server, external to the application (hence external to the server)

## 2. In-process Caching
- Stored in the application server itself. 
- Each application server contains its own cache. It's faster for the application to use, but the cache is stored locally to that application server, so other's cannot access it. This can cause inconsistency

## 3. Content Delivery Network (CDN)
- A CDN ensures that requests from users hit servers close to the user, meaning the latency remains low. 
- This is why loading some content sometimes takes a while - because it's stored on some server far away, not in the local edge server. 
- Optimises for latency. 

## 4. Client-side Caching 
- Data is stored on a user's device, not necessarily in RAM but it does remain easily accessible. 

# Cache Architectures
## 1. Cache-aside
- A dedicated cache server like Redis is hosted, separately to the database. 
## 2. Write-through Caching
- App server -> Cache -> Database
- This requires something to handle the write-throughs between app servers/cache. 
## 3. Write-behind Caching
- App server -> Cache -> Database
- But with write behind, there is an asynchronous flush to the database. 
- Necessary for high write-through systems. 
## 4. Read-through Caching
- Similar to [[#1. Cache-aside|cache aside]] 

# Cache Eviction
### Least Recently Used (LRU)
Evict items that haven't been used recently - most common and balanced. 
### Least Frequently Used (LFU)
Evict items that are used least often, even if accessed recently.
### First-in First-out (FIFO)
Evict the oldest item first. This is simple, but often **not** the right choice. 
### Time-to-live (TTL)
Each item expires after a set time. Useful for data that can can go stale, such as [[Application Programming Interface (API)|API]] responses. 


# Common Issues
## 1. Cache Stampede (AKA Thundering Herd)
- A cache miss can lead to the database being overwhelmed. 
### Prevention
- Request coalescing. 
- Cache warming. 

## 2. Cache Consistency
- Stale data in the cache. 
- Data that is updated in the database is not updated in the cache. 
- Related to [[CAP Theorem#Consistency|CAP Theorem]]. 

## Hot Keys
- Related to Cache keys. 
- Issue occurs when having to shard databases. 


# When is Caching Relevant?
- Read-heavy workloads
- Expensive queries
- High [[Database|database]] CPU load
- Latency requirements

How to think about introducing caching
- Identify bottlenecks
- Decide what to cache
- Choose the cache architecture
- Set an eviction policy
- Address the drawbacks


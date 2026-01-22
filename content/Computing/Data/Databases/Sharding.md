---
aliases:
  - sharding
---
# Why Shard a Database?
- When you outgrow a database. 
	- You need more writes/second. 
	- Storage is approaching a limit
	- Database in general is straining. 
- Sharding is useful to *scale horizontally*, meaning more databases rather than bigger databases. 

# What is Sharding
- Splitting the data across multiple databases. 
- The shards together form the full dataset. 
- As you grow, you add more shards, not larger databases. 

# How to Shard Data
This raises questions:
1. What to shard by (shard key - how to group data)
2. How to distribute data ()

## Choosing Shard Keys
### Good shard keys
1. High cardinality (field with lots of unique values)
2. even distribution (values should naturally spread, so each shard has an even amount of data)
3.  aligns with queries (1 query hits one shard)
### Good examples
1. Split by user ID (shard 1 has users 0, 10m; shard 2 has users 10m, 20m; etc)
2. Split by order ID (shard 1 has orders 0, 10m; shard 2 has orders 10m, 20m; etc)
3. Think about what users are requesting frequently. 

## What to Distribute By
1. Range-based (user IDs, order IDs)
2. Hash-based distribution. 
	1. Take a uniqueID, hash it (to randomise) and run a modulus operation on how many databases you have. The problem is, if you add a new shard. 
3. Consistent-hashing. 
	1. The coin spinning analogy. 
	2. Eliminates the giant reshuffling problem. 
4. Directory-based Sharding


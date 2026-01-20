
## 1. Networking Approaches
Understanding the different [[Protocols]] is important when considering your system's architecture. Since these systems will be providing services over the internet, the standard is *HTTP over TCP* given it's widespread use and common understanding with developers. 
### Load Balancing and Networking
You can choose to balance loads at different stages of the [[Networking 101#The OSI Model|OSI model]] - the earlier you load balance, the more information you retain about the data that's being sent, granting you the ability to balance loads based on the content of what's being transmitted. 

In [[Networking 101#Layer 7|Layer 7]] of the OSI model is where you are able to see the information being sent and hence are able to make better load balancing decisions. 
## 2. [[Application Programming Interface (API)|API]] Design
API design is about creating reasonable endpoints. 

## 3. Data Modelling
The main question when considering how to model your data is Relational (SQL) or Not Relational (NoSQL). Relational databases are most useful when you prioritise data [[CAP Theorem|Consistency]], since relational databases require relationships between tables, hence they lend themselves to consistency. NoSQL databases like DynamoDB or MongoDB shine when you need flexible schemas (your data structure changes frequently) or you need to scale horizontally across many servers without complex joins.



## 4. Database Indexing
## 5. Caching
## 6. Sharding
## 7. Consistent Hashing
## 8. CAP Theorem

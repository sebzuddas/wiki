
## 1. Networking Approaches
Understanding the different [[Protocols]] is important when considering your system's architecture. Since these systems will be providing services over the internet, the standard is *HTTP over TCP* given it's widespread use and common understanding with developers. 

### Load Balancing and Networking
You can choose to balance loads at different stages of the [[Networking 101#The OSI Model|OSI model]] - the earlier you load balance, the more information you retain about the data that's being sent, granting you the ability to balance loads based on the content of what's being transmitted. 

In [[Networking 101#Layer 7|Layer 7]] of the OSI model is where you are able to see the information being sent and hence are able to make better load balancing decisions. 
## 2. API Design
## 3. Data Modelling
## 4. Database Indexing
## 5. Caching
## 6. Sharding
## 7. Consistent Hashing
## 8. CAP Theorem

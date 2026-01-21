---
aliases:
  - Consistency
  - Availability
  - Partition Tolerance
tags:
  - computing
  - data
---
CAP theorem describes the tradeoff between Consistency, Availability and Partition Tolerance of data. [[#Partition Tolerance]] is often unquestionable, so the tradeoff becomes between [[#Consistency]] and [[#Availability]]. 

More on CAP: [here](https://www.ibm.com/think/topics/cap-theorem) and [here](https://en.wikipedia.org/wiki/CAP_theorem). 
# Consistency
Consistency is important when every [[Database|database]] write requires a subsequent database read. Systems that depend on this mode of operation with their data must ensure data consistency across their systems. 

# Availability
Availability is important when ...

# Partition Tolerance
Partition tolerance is almost always non-negotiable in the context of a large-scale software application that has users across the world. This is because these systems are distributed, and the partitioning of data has to happen to account for latency. 


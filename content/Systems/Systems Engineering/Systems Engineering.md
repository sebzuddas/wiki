---
aliases:
  - SE
  - Systems engineering
  - systems engineering
tags:
  - complexity
  - engineering
  - system
  - systems
---
# What is Systems Engineering
Systems engineering is the discipline that brings together multiple engineering fields in order to deliver complex engineering projects and products. It is an interdisciplinary field that intersects technology and management. 

# Why do we need Systems Engineering
Systems engineering is necessary for engineered systems that have a level of complexity in which a small group of specialised engineers cannot themselves manage. The core function of systems engineering processes is to handle complex projects and ensure that the system itself delivers on the capabilities it promises. The more complex a system, the higher the RoI in terms of value that systems engineering provides - both in terms of money and time. 

# How do I Engineer Systems
There are two notable approaches to following systems engineering processes, notably [[#Document-based Systems Engineering]] and [[#Model Based Systems Engineering (MBSE)]]. When implementing these approaches, the process typically follows a specific [[#Development Models|development model]]. **There is no one correct way to do systems engineering**, since each project/product has its own unique needs. Delivering 100 trains is different to delivering a robust telecommunication software. Software developers may initially jump to using agile, 

---
# Development Models
## Traditional V-Model

![image](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e8/Systems_Engineering_Process_II.svg/2560px-Systems_Engineering_Process_II.svg.png)
## Agile V-model

![agile v-model](https://aiotplaybook.org/images/thumb/c/c2/2.1-AgileV.png/1600px-2.1-AgileV.png)


## Pure Agile

![agile development process](https://www.rosemet.com/wp-content/uploads/2024/07/WHATAR1.jpg)

## Spiral Model

![spiral model](https://e99wnqo63x9.exactdn.com/wp-content/uploads/2011/08/Boehm-Spiral-Model.jpg?strip=all&lossy=1&w=2560&ssl=1)

---
# Document-based Systems Engineering
Disclaimer, [NASA's systems engineering handbook](https://www.nasa.gov/wp-content/uploads/2018/09/nasa_systems_engineering_handbook_0.pdf) is the go-to reference for document-based systems engineering. Similarly, ESA has [useful documentation](https://ecss.nl/standard/ecss-e-st-10c-rev-1-system-engineering-general-requirements-15-february-2017/). The intention of this section is to simplify these large documents and have some 'ready-to-go' templates. Markdown is the chosen format since it encourages *writing* as opposed to *document design*. You can then use a [markdown to pdf converter](https://www.markdowntopdf.com/), or simply copy and paste into your chosen word processor. 

## 1. Concept of Operations (ConOps)
The concept of operations is a document that outlines the functionality and interactions of the system at a high-level. It is meant to be a point of reference for teams to then delve further into technical details. Find a template [[1. Concept of Operations|here]]. 

## 2. Requirements and Architecture
System requirements are define the specifics of how a system should operate. Requirements should be detailed enough for engineers of different disciplines to create technical design documentation based from. There are known approaches to writing good system requirements, which you can find [here](https://www.nasa.gov/reference/appendix-c-how-to-write-a-good-requirement/). You can find a requirements template [[2. System Requirements|here]]. 

High-level system architectures are a way to help understand how the system is structured, typically through diagrams. There is a conceptual overlap with [[Model Based Systems Engineering (MBSE)|MBSE]] approaches in this phase. 



## 3. Detailed Design


## 4. Implementation


## 5. Validation and Verification
**Validation** is defined as: the assurance that a product, service, or system meets the needs of the customer and other identified stakeholders. It often involves acceptance and suitability with external customers. **In other words** - does the system work as intended based on what the user asked for? "Are you building the right thing?"

**Verification** is defined as: the evaluation of whether or not a product, service, or system complies with a regulation, requirement, specification, or imposed condition. It is often an internal process. **In other words** - is the built system in line with requirements (and regulations)? "Are you building it right?"

## 6. Operation and Maintenance


---
# [[Model Based Systems Engineering (MBSE)]]

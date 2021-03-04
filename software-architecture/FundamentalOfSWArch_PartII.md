# Architecture Styles

**S/W Architecture** - overarching structure of how the user interface and backend source code are organized and how that source code interacts with a datastore
_Concentrate on the *tradeOffs* associated with each style_

**S/W Pattern** - are lower-level design structures that help form specific solutions within an architecture style

## Chap 9 - Foundations

Architecture Style defines

* topology
* assumed & default architecture characteristics
  * beneficial & detrimental *

### Fundamental Patterns

These are patterns which are found across styles.

1. **Big ball of Mud**

* Problems
  * deployment
  * testability
  * scalability
  * performance

2. **Unitary Architecture**
   Unitary = H/W + S/W

* Problems
  * No seperation of concerns
  * Performance
  * scale

3. ** Client/Server**
    Partitioning from single system
    Types of Client/Server systems
    1. Desktop + Database Server
    2. Browser + WebServer 
       - Seperate database server still considered 2-tier arch(basis : type of machine).
    3. Three Tier
        Additional Application Server + Javascript with html.

        **Note:** Interesting note about how serialization was built into java as object passing between layers was considered an important concern.
        *ADVICE:*Favor **simple design** in defence for future consequences. 

### Monolith vs Distributed Architecture
 **Monolith**: single deployment unit of all code.

 * Layered Architecture
 * Pipeline Architecture
 * Microkernel Architecture

 **Distributed**: multiple deployment units connected through remote access protocols
 
 * Service Based Arch
 * Event-driven Arch (EDA)
 * Space based Arch
 * Service Oriented Arch
 * Microservices Arch

 **TradeOff** 

 * Fallacies of Distributed Computing https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing - automatic!
 
 1. The network is reliable
    more a system relies on the network (such as microservices architecture), the potentially less reliable it becomes.
 2. Latency is zero;
    Should Know latency average in network
    95th & 99th percentile also (long tail kills performance)
 3. Bandwidth is infinite;
    > higher comm increase data transfer
    increases latency & reduces reliability
    **stamp coupling**: consumes significant amounts of bandwidth
    Resolved by:

     * Create private RESTful API endpoints
     * Use field selectors in the contract
     * Use GraphQL to decouple contracts
     * Use value-driven contracts with consumer-driven contracts (CDCs)
     * Use internal messaging endpoints
     ADVICE : minimal amount of data is passed between services 
 4. The network is secure;
    * surface area for threats and attacks increases by magnitudes when moving from a monolithic to a distributed architecture
    * Having to secure every endpoint, even when doing interservice communication, is another reason performance tends to be slower in synchronous, highly-distributed architectures such as microservices or service-based architecture
 5. Topology doesn't change;
    Architects must be in constant communication with operations and network administrators to know what is changing
 6. There is one administrator;
 7. Transport cost is zero;
    Transport cost - actual cost in terms of money associated with making a “simple RESTful call"
    Distributed architectures cost significantly more than monolithic architectures, primarily due to increased needs for additional hardware, servers, gateways, firewalls, new subnets, proxies, and so on.
 8. The network is homogeneous.
    not all of those heterogeneous hardware vendors play together well. Leading to unrealiable, latency, bandwith issues.
 9. We all trust each other. -- redundant but mentioned on wikipedia...

 * Other distributed Considerations: 
  * Distributed Logging 
  * Distribted transactions 
    * eventual consistency
    * Transactoinal sagas
        Sagas utilize either event sourcing for compensation or finite state machines to manage the state of transaction
    * BASE :  (B)asic availability, (S)oft state, and (E)ventual consistency
        Soft state in BASE refers to the transit of data from a source to a target, as well as the inconsistency between data sources. Based on the basic availability of the systems or services involved, the systems will eventually become consistent through the use of architecture patterns and messaging.
  * Contract maintainence & versioning
    version deprecation even more complex.

 


### Why Design Pattern?
- Software template to solve a problem that occurs in multiple instances while designing an application.
- If multiple developers are working on the project. Everyone builds in their own way. Which is not maintainable. If we follow some set of design patterns to build an application which is already solved different issues and is reliable. We need to force users to be on the same page.

### MS Architecture:
- The architecture style is a collection of small services, each with a single business responsibility.
- All Microservices serve the purpose of business Domain.

### Principle/ Advantages:
- Independent & Autonoumous Service
- Scalability: Scale individual Service
- Decentralization: All control is divided into individual services
- Resilient Service: If a single Service goes down, All Applications will not go down.
- Load Balancing:
- Availability:

Cons:
- High Cost
- Architcture becomes complex, So More developer requires

## Design Patterns

1. Aggregator:
- This pattern is used If a client needs data from two MS.
- Create a Separate MS which calls diff microservice, and combines data based on requirements and responses.

2. API GATEWAY:
- Client -> API GateWay -> Load Balancer -> Service 1, Service 2…

3. Chained or Chain of Responsibility:
- Produces single output which is a combination of multiple chained outputs.
- MS1 -> MS2 -> MS3: MS calls one after another. The response of MS3 is the final result produced which is initiated at MS1.

4. Asynchronous Messaging:
- All Services can communicate with each other, They do not have to communicate with each other sequentially.

5. Database:
- Database per Service
- shared Database

6. Event Sourcing:
- Create an event regarding the changes in mIcroservices.
- Any past changes in the state can be published and viewed. Just like changing the log, or changing the history. (When, by whom, what etc)

7. Branch:
- Simultaneously process the request and response from multiple independent Microservices.

8. Command Query Responsibility Segregator (CQRS):
- The application is divided into two part 
  - Command: Handled all requests related to CREATE, UPDATE, DELETE
  - Query: READ

9. Circuit Breaker:
- Design pattern to deal with non-transient failure in microservices.
- Circuit Breaker Pattern blocks calls to failing MS and allows MS to recover.
- 3 states: Closed -> Open -> Half-Open 
  - Closed: Default state where CB allow communication between MS
  - Half-Open: Recovering: Here some traffic can go from A to B (failing) for testing purposes. To ensure failing service is working correctly.
  - Open: It means all traffic to failing MS is blocked

## Concepts/ Terms:
### Non-transient failure
Permanent failure which can make the system unavailable or recovery can take longer than a few seconds
Ex: Api unavailable, DB down,

### Cascading failure:
If multiple chained/dependent MS are failing due to one MS is known as Cascading failure.
MS1 -> MS2 -> MS3, If MS3 is down. All MS are dependent MS3 will get affected

### Resilient System

### Thundering herds

### Retries and recovery



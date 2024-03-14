Distrubuted Design pattern- https://www.youtube.com/watch?v=nH4qjmP2KEE

## SAGA Design Pattern
- For example, If we call multiple dependent microservices and one of it is failed them either everything has to rollback or needs to perform some failed operation on it. For such cases, the SAGA Pattern is used.

<img width="1137" alt="Screenshot 2024-03-15 at 2 54 48 AM" src="https://github.com/preetampatil3d/What-I-Learned/assets/21255598/6b12e0f9-00e0-445e-85df-6a5a1c2d39ea">

- Every MicroService should have failure code along with regular or happy code flow. So that it will called in case of failure
- In Case of failure at one Microservice , It will trigger each MS in reverse order till starting point for Failure Code execution.
> [!NOTE]
> This is not roll back, This Revert.
> For example payment MS, Payment will be reverted or send back to customer.

### Two Ways:
1. Choreography 
- Coordinate with a single messaging broker.
- ie. The Client will send a message to the Broker, Then the Messaging Broker will communicate the required MS. Also triggered MicroService will communicate with other Microservice through the Messaging Broker only by sending a required message (start MS2)

<img width="1075" alt="Screenshot 2024-03-15 at 3 22 27 AM" src="https://github.com/preetampatil3d/What-I-Learned/assets/21255598/11c87ecb-fc05-459a-9b3d-3139dc50f373">

- Cons:
  - Sometimes it confusing for who is listening to what, backtracking is difficult
  - Cyclic dependecy: 1st is waiting for 2nd command and 2nd is waiting for 1st command

2. Orchestration
- Orchestration handles all the transaction and tell each participant to which operations to perform based on event.
- Task of Orchestration
  - Exceute the request
  - Store state of each task/request. If task success -> trigger next or revert till start. (with failure/compansation code) 
  - handle failure by executing related failure code from MS.

<img width="1093" alt="Screenshot 2024-03-15 at 3 31 46 AM" src="https://github.com/preetampatil3d/What-I-Learned/assets/21255598/49227aad-708c-4441-b03e-53f5ad675c0a">

- pros:
  - No Cyclic Dependency
  - Good for complex / many Participants/MS.
 
- Cons:
  - Additional complexity for implementing co-ordination logic
  -    
  

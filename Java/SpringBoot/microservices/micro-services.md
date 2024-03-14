# Microservices:

> www.youtube.com/watch?v=lTAcCNbJ7KE

- Monolithic: The application is built in a single package/Unit.

- Micro-Services: Microservices are loosely coupled Architecture inside large-scale applications.

- Communication: MicroServices talk to each other through 
	- Remote Procedure Call (RPC) - gRPC: Provide faster response, But Impact would be larger when MicroService Go Down
	- Event Streaming - Better Isolation but it takes a longer time to process
	- Message Brokers 

- Databases: Well-architected micro-services practice Strong information hiding, which means breaking up monolithic Databases specific into individual Microservice
	- Cons: Breaking up databases will loss of foreign key connections and need to manage this at the Application layer (individual micro-service). 

- API Gateway: Handles incoming requests and routes them to requested micro-services
- To Route to correct MS, Api-Gateway consults Service-Registry and Discovery Service

- Service-Registry & Discovery Service:  MS need to register with Service-Registry and Discover the  Other services through service-registry Service

- When to use:
  - For larger applications as it requires more cost and manpower to handle multiple MS

> Note: Startup can design each function in the application with a well-defined interface so that in future it would be easier to migrate.


# Simple MS
- Create Clients / MS
  - Maven Dependecy req : spring-cloud-starter-netflix-eureka-client
  - application.properties
```
    server.port=8081 # set different port for each MicroService
    application.name=USER-SERVICE # Set Unique Name for Each MS, It can be used to call MS
```

- Create Eureka Server:
  - Maven Dependecy : spring-cloud-starter-netflix-eureka-server
  - application.proerties
  ```   
  server.port: 8761 # If we change port apart from default '8761'. need to add in all services 
  eureka.client.fetch-registry=false
  eureka.client.register-with-eureka=false
  ```

- Communication using REST api
  - Create Bean for RestTemplate to call api
  ```
  @Bean
	@LoadBalanced // used if we want to call other MS using MS ID
	public RestTemplate getRestTemplate() {
		return new RestTemplate();
	}
  ```
  - Call other MS
  ```
  List<Users> list = restTemplate.getForObject("http://USER-SERVICE/api/getall", List.class);
  ```


## Fault tolerance & Circuit Breaker
- Fault tolerance: Fault Tolerance is a property that enables the system to work properly even though some connected Component/MS are down
> Fault: If any MicroService is down or not reachable then it is called a fault.
	
### Circuit Breaker: is a design pattern which allows microservices to be operational when a connected microservice fails or down
- Wrap Rest Method with circuit breaker Object and monitor for failure.
- Once failure reaches its threshold, the Circuit breaker trips and all further calls return failure with an error. 
- For the above case, the fallback method is executed once the circuit breaks or open
- Circuit breaker states 
	- Closed: Normal state - Circuit breaker only monitors for failure and allows the request to pass
	- Open: Block communication - It will completely block communication with other MS. Once time out period is completed,  It will move to Half Open.
	- Half Open: Recovery period - Allow limited requests to pass to evaluate whether MS is recovered or not.

### Configuration for Hystrix (Deprecated) -> Now resilience4j is used
- Maven Dependency: spring-cloud-starter-netflix-hystrix
- Add @EnableCircuitBreaker to the main class (Note: is deprecated)
- Add @HystrixCommand(fallbackMethod = “”) to Rest method which might fail due to connected MS call
- Create fallbackMethod with the same signature as Rest Method where we declared @HystrixCommand
```
@GetMapping("/getallempusers/{id}")
@HystrixCommand(fallbackMethod = "fallbackMethodGetAllInclingUser")
public ResponseEntity<RequiredResponse> getallIncludingUser(@PathVariable Long id) {
	Employer emp = service.getById(id);
	RequiredResponse req = new RequiredResponse();
	req.setEmployer(emp);

	List<Users> list = restTemplate.getForObject("http://USER-SERVICE/api/getall", List.class);
	req.setUsers(list);

	return new ResponseEntity<RequiredResponse>(req, HttpStatus.OK);
	}
	
public ResponseEntity<RequiredResponse> fallbackMethodGetAllInclingUser(@PathVariable Long id) {
	Employer emp = service.getById(id);
	RequiredResponse req = new RequiredResponse();
	req.setEmployer(emp);
	return new ResponseEntity<RequiredResponse>(req, HttpStatus.OK);
	}
```

## API Gateway
- An abstraction layer between Front-End and Back-End Developers. 
- Centralized common functionality in one place like authentication, logging, and Authorization. So individual microservices need not be required to do that.
- Can call Multiple Versions of MS can be used. This can be done using the API Gateway Routing Technique.
- Available Options 
	- Netflix API Gateway (Zuul)
	- Spring Cloud Gateway
> Most Netflix libs related to MS are now in maintenance mode so they will not be having any new features or the next version.
>  Available Netflix libs: Zuul, Hystrix, Eureka. But still, Eureka is used widely.

### Spring Cloud Gateway
- Maven Dependency :  
- spring-cloud-starter-gateway
- spring-cloud-starter-netflix-eureka-client
- application.property
```
server.port: 808
spring:
  application:
    name: API_GATEWAY
  cloud:
    gateway:
      routes:
      - id: USER-SERVICE
        uri:
          lb://USER-SERVICE
        predicates:
        - Path=/user/**
      - id: EMPLOYER-SERVICE
        uri:
          lb://EMPLOYER-SERVICE
        predicates:
        - Path=/employer/**
```
- Now, we can use the URI of the gateway and call configured MS. Just change the port of the Gateway
- Uri: if not used then we need to provide the full URL
- Predicates: If path is match then only redirect to lb path

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



2. Orchestration 



    

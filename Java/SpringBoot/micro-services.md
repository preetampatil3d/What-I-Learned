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


    

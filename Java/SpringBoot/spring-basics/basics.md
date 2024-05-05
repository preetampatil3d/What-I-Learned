### Spring Boot Features
- AutoConfiguration
	- If we add any dependency through maven, It will autoconfigure it and be ready for use, Like if we add JPA. It will configure everything needed for it
- No @ComponentScan
- Starter Projects
- Actuator
- Embedded Server
- Support for integration of the framework

### @SpringBootApplication
- @SpringBootConfiguration - this creates AppContext just like @configuration
- @EnableAutoConfiguration - 
- @ComponentScan - Scan the current package

### @EnableAutoConfiguration
- Whenever we add any dependency through jar/maven. Spring looks for existing configurations for specific applications (JPA Data). Based on this Spring Boot provides the basic configuration needed to configure.

### Dependency Injection
1. Setter Injection
	- Injection happens through the setter method. Add @Autowired on top of the setter
2. Constructor Injection: Prefered as per spring documentation 
	- Injection happens through the constructor. Add @Autowired on top of the constructor
	- Faster as it loaded at the time of bean initialization.
3. Field Injection
	- Add @Autowired on top of the field directly. Spring will use reflection to wire this bean and inject.


### Setter vs Constructor
| Setter | Constructor |
| --- | --- |
| slower than Constructor | faster, injected at time of initialization |
| Use for Optional dependencies | Use of Mandetory Dependencies |
| | Preferered By Spring |

### Component vs Controller vs Service vs Repository 

| Component | Controller | Service | Repository |
| --- | --- | --- | --- |
| Parent of other anotation / Generic | MVC | Business Layer | Data Layer |
| | | | It will add JDBC exception |

### Spring Application Context
```
// Define/Declare ApplicationContext
@Configuration
class SpringContext{
}
// Usage : Spring
ApplicationContext cntx = AnnotationConfigApplicationContext(SpringContext.class);

// Usage : Spring Boot
@SpringBootApplication // Everything will be handled by spring automatically 
public class SpringBootDemoApp{
}
```

### Autowiring
1. ByType: matches with field type
  - If Autowired Bean is a class it looks for a specific class
  - If it is an interface it looks for its implementation class
2. ByName
  - If the Interface has multiple interfaces, then it will match its name
```
// Declaration and implementation
Public interface Algo{
}
@Component
Public class QuickSortAlgo implements Algo{}
@Component
Public class BubbleSortAlgo implements Algo{}

// Usage
@Autowired
Private Algo bubbleSortAlgo; // It will inject a bean of bubbleSort 
```
3. By Constructor
  - Same like ‘Type’ but through a constructor
 
### @Primary , @Qualifier 
If two matching implementations of the interface are present spring will through NoUniqueBeanException
To solve this we can use the following options
- Proper Naming: We can name it properly with the implementation class which we want to inject.
- @Primary: using @Primary along with @Component. So that it will picked in such a case.
- @Qualifier: Bean will injected based on name provided in qualifier
	```
	@Component
	@Qualifier(“customName”)
	public class QuickSortAlgo implements Algo{}

	@Autowired
	@Qualifier(“customName”)
	QuickSortAlgo demoBean;	
	```

### Exclude embeded tomcat and use jetty
```
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
	<dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-starter-jetty</artifactId>
	</dependency>
</dependencies>
```

### @ConfigurationProperties - Configure and use Custom properties
```
@Component
@ConfigurationProperties("basic")
class BasicProperties{
	String name; // name from property will be mapped here
	String email; // email from property will be mapped here
}
// Usage
@Autowired
BasicProperties basicProp;

// Application.properties
basic.name=
basic.email=
```

### Profiles: Create and set property file for individual environment
```
// Create env specific properties
application-dev.properties
application-prod.properties
// Default property file where we defile active property
application.properties
spring.profile.active=prod

// Using cmd
Dspring.profile.active=prod
```

### SpringBootActuator
- To use it, we need to add its Maven dependency
- Usage
	- Base path  /application
	- /env: show all the info related to env where the application is deployed and versions.
	- /autoconfiguration: PositiveMatches, NegitiveMatches
	- /metrics: details related to, heap, thread, gc, processes, sessions: number of errors encountered, number of success request
	- /beans: beans Configured
	- /mapping: Rest URL mapping


## Runner interfaces (Functional Interface / Similar to Static Block): 
- One-time execution Code is placed in run() of Runner Bean Class.
- If the Application has two CommandLine & Application implementations present the CL Runner will have high-priority 
- If the Application has multiple Runner implementations  of the same type then it will be alphabetically executed
- We can set order by annotating the Runner class with @order(10)
	- Additionally, implement the Ordered interface to the Runner class and override getOrdered(){ return 0;}
**Types:**
1. CommandLineRunner:
- Get all Optional & no-Optional arguments in the String[] array
```
// Argument: 11 str123 - - name=Preetam
// 11 , str123 : non optional arguments
// - - name=Preetam : Optional arguments
@Component
Public class DemoRunner implements CommandLineRunner{
	// Injections. . .
	@Override
	public void run(String… args) throws Exception{
		//One-time code
		System.out.println(Arrays.toString(args));
	}
}
```
2. ApplicationRunner
- Get all Optional & no-Optional arguments in ApplicationArguments Object.
```
Public class DemoRunner implements ApplicationRunner{
	// Injections. . .
	@Override
	public void run(ApplicationArguments args) throws Exception{
		//One-time code
		List<String> nonOptionArgs = args.getNonOptionArgs();
		Set<String> optionArgs = args.getOptionNames();
	}
}
```

> [!NOTE]
>  Static blocks are used for one-time execution,  Then why Runner?

Diff between static block & Runner
| Static Block | Runner |
| --- | --- |
| Cannot use non-static data | Can access static data in Runner |
| It does not allow passing values from outside | Allos to pass data/Args to run(), command line argument can be passed / Inject Bean |
| It does not support exception propagation | It supports Exception handling | 











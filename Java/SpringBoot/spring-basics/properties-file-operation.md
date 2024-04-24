## Read / Set Properties
### Set Properties
Add key=value in below option
1. Add in properties file
2. Set through Cmd variable: java -jar myApp.jar - - server.port:8080
3. Set in ENV variable of System - In IDE, VM Option :  -Dserver.port=8080

### Read Properties from a properties file
1. Using @value() : Read Single field
```
@Value(“${System.env.ip}”) // This can be read from Cmd, Env, Properties, 
String ip;
```
2. Using @ConfigurationProperties : Bulk Read
```
# application.properties
system.env.ip=
system.env.instanceName=
system.env.type=

#
@ConfigurationProperties(prefix=“system.env”)
Public class ConfigPropertyies{
	String ip;
	String instanceName;
	String type;
	// Setter, getter
} 
```
> When I have both application.properties & application.yml, which one will load
- yml file will take precedence 

### Precedence order of properties
1. Command Line Argument
2. Environment Variable
3. Application.yaml
4. Application.properties

### Create External Property file
- java -jar app.jar - - spring.config.location = /path/to/config.properties

### Multiple custom property files
```
# application.yml
Spring:
  Config:
    import:
      - info.yml
      - info1.properties
```


## Profiles
### Creating multiple property files for individuals environment
- Create property files
  - application-<ENV-NAME>.properties
  - application-<ENV-NAME>.yml
- Set/Activate current profile
  - Using Property file: spring.profile.active=dev
  - Using cmd: —spring.profile.active=dev 

### Enable specific Bean/Repository to be used for specific environment.
```
@Repository
@profile(“dev”)
Class MysqlEmployeeDao implements IEmpDAO{}

@Repository
@profile(“uat”,”prod”)
Class OracleEmployeeDao implements IEmpDAO{}

@Bean
@Profile(“test”)
Class TestBean{}
```

### In a single yml file, we can define multiple profiles by separating them with “- - -”
```
# Common for all profile
logging.level:
  org.springframework: INFO
…

---
# Dev Profile
spring.prifile: dev
Spring
 Datasource
   Url:mysql

---
# Prod Profile
spring.prifile: prod
Spring
 Datasource
   Url:oracle

```

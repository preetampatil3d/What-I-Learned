## Connection pool: Hikari

- Connection Pool is created by the container to manage and reuse connections.
- Hikari is imported with spring-boot-starter-data-jpa or spring-boot-starter-project
- With SpringBoot > 2.0, No need to add a dependency for Hikari.

Old ways:
- Earlier tomcat separately maintained this connection Pool
- We need to add hikari depedency manually in pom
  
```
# Application.properties.yml
Spring
  datasource
    hikari
      Connection-timeout:50000 // ms
      Minimum-idele:5
      Maximum-pool-size:10
      Max-lifetime=1200000
      Auto-commit:false
```

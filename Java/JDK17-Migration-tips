## debug tips: 
### Update JDK version in pom
- Update the maven-compiler-plugin configuration in your pom.xml file to specify the new JDK version. For example, if you are migrating to JDK 17, you would set the source and target versions to 17:

### Migarte all javax import to jakarta
- Use IDE to find all javax import and change to jakarta

### for version conflicts, use dep tree
mvn dependency:tree -Dinclude=org.springframework.data:spring-data-rest-webmvc
mvn dependency:tree -Dinclide=com.atomic -Dverbose

### generate effective pom
- mvn help:effective-pom
- We can do from any ide

### spring batch
- spring batch 5.0.0 is compatible with spring framework 6.0.0 and spring boot 3.0.0.

### https client5
- HttpClient 5.0.0 is compatible with Java 11 and later versions, including Java 17. It is recommended to use the latest version of HttpClient to take advantage of the latest features and improvements.


# Custom Queries:
### JPQL: Create Query based on Entity model classes
- Not fit for complex queries as it difficult to create queries

- Usage - @Query
```
// Service class
Sort sort = new Sort(Direction.ASC,’firstName’);
Pageable pageable = new Pageable(0,10);

Public interface EmployeeRepository extends JpaRepository<Employee,Long>{
	@Query(“from Employee where firstName = ?1”)
	List<Employee> findByFirstName(String firstName, Sort sort,Pageable pageable );

	@Query(“from #{#entityName} where firstName = ?1”) // name will be taken from Repository entity

	// In Order to Perform Operations like DELETE, INSERT, UPDATE and DDL we need to annotate with @Modifying
	@Modifying
	@Query("update Employee e set e.active = false where u.lastLoginDate < ?1")
	void deactivateEmployeeNotLoggedInSince(LocalDate date);
}

```

### Native Queries: 
- Uses bind parameter, It prevent SQL Injections
-  Usage
```
Public interface EmployeeRepository extends JpaRepository<Employee,Long>{
	@Query(“select * from Employee where firstName = :firstName”, nativeQuery = true)
	List<Employee> findByFirstName(@param(“firstName”) String firstName);

	@Query(“from #{#entityName} where firstName = ?1”) // name will be taken from Repository entity
}

```

> Bind Parameter
1. Name Bind Parameter:
```
“firstName = :firstName”
findByName(@param(“firstName”) String name)
```
2. Position Bind Parameter
```
“firstName= ?1”
findByName(String name)
```


### Named Query: @NamedQuery
- Allow to declare at the Entity/Persistence layer and use it directly in business code

- Using JPQL
```
@NamedQuery(
	name = “Employee.findByFirstName”,
	query= “From Employee where firstName = ?1”
)
@NamedQuery(
	name=“Employee.findByLastName”,
	query=“From Employee where lastName = ?1”
)
@Entity
Class Employee{
}
```
- Using Native Query: @NamedNaiveQuery
```
@NamedNativeQuery(
	name=“Employee.findByFirstName”,
	query=“Select * from employee where firstname = ?”, // position not required
	resultClass = Employee.class
)
@Entity
Class Employee{
}
```

- Way 1 - Call Named/NativeNamedQuery:
```
@Autowire
EntityManager em;

BusinessMethod(){
	Query q = em.createdNamedQuery(Employee.findByFirstName); // name declared at @NamedQuery in Entity
	q.setParameter(1,”TestName”);
	List<Employee> empList = q.getReSultSet();
}
```
- Way 2 - Call By declaring in the Repository
```
@Repository
Interface EmployeeRepo extends JpaRepository<Employee,Long>{
	List<Employee> findByFirstName(String firstName);	
}

@Service
Public class EmployeeService{
	@Autowired
	EmployeeRepo repo;
	
	public businessMethod(){
		List<Employee> list = repo.findByFirstName(“testName”);
	}
}
```

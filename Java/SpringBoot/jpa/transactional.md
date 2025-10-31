## @Transactional -
- Can be declared on a method where we are going to perform multiple db-related changes. @Transaction ensured all data persisted at the end of the method. So that in case of any exception in between everything will be rolled back
- We can use @Transactional for level - Class, Method, interface, 
- If we add @Transaction at class & method. Method will be given preference.

### Properties:
- Propagation: Used to create new or existing transactions. The default is to use existing transactions.
- When @transactional is added to the inner or nested method. It uses the same transaction as a parent. To change that we can make use of propagation.
```
@Transactional(propogation = Propagation.REQUIRED)
// Propagation.REQUIRES_NEW // new transaction created
// 
```

## Working:
- Create a Proxy or 
- Manipulate the ByteCode by wrapping some spring code to handle transaction and rollback.
```
createTransactionIfNecessary(); 
try { 
	addEmployee(); 
	commitTransactionAfterReturning(); } 
catch (exception) 
{ 
	rollbackTransactionAfterThrowing(); 
	throw exception; 
}
```
### ACID - Ensures ACID character.
- Atomicity - All or nothing principle. Perform all operations or nothing.
- Consistency - Ensures all operations rolled back to the initial state from where they started, Including all constraint checks.
- Isolation - Transactions performed within a transaction are not visible to other transactions.
- Durability - Ensures committed changes persisted.

  

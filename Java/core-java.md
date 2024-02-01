## String/ array of character/ Immutable Object
- String is refered as immutable because its value cannot be changed. 
	- Instead whenever value changed, new obeject is created and old value is copied. To prevent this we can make it ''
- All Warpper classes are immutable and Thread safe.
- Example
String a1 = "Hi"; // Stored at new memory/String Pool
String a2 = "Hi"; // Search for 'Hi' in String Pool and points to previously created pool.
String a3 = new String("Hi"); // It will Not search in Pool instead it will be stored at new memory location.
- Mutable String : Create String using StringBuilder(Not Thread-Safe) and StringBuffer (Thread-Safe).
- Advantages : 
	- Security: sensitive data like username, password, connection can be stored and which can not be manupulated by hackers.
	- Sychronization: Can be shared with other threads and be asured data can not be modified
	- Performance: Due to String Pool

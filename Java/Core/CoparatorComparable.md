| Comparator | Comparable |
| --- | --- |
| External Sort | Natural Sort |
| Usage: Create Comparator Obj and pass for sorting | Usage: Add Sorting logic in class itself |
| Compare two consecutive ele for sort | compare with itself for sort |
|  |  |

## Coparator
- Comparator will do bubble sorting internally So we should return below value based on specific condition.
  - Return 1 : swap
  - Return -1 : Do nothing
 
- Example of list of  Object	
```
// Sort by Empployee Id & dob
List<Employee> list = new ArrayList<>();
list.add(new Employee(1, "B", LocalDate.of(1990, 01, 01) , 34));
list.add(new Employee(2, "D", LocalDate.of(1985, 01, 01) , 39));
		
list.add(new Employee(3, "E", LocalDate.of(1995, 01, 01) , 29));
list.add(new Employee(3, "F", LocalDate.of(1996, 01, 01) , 28));
list.add(new Employee(3, "G", LocalDate.of(1994, 01, 01) , 31));
		
list.add(new Employee(4, "A", LocalDate.of(2000, 01, 01) , 24));
		
		
// Using Comparator
Comparator<Employee> firstComp = new Comparator<Employee>() {
  @Override
  public int compare(Employee o1, Employee o2) {
				if(o1.getId() > o2.getId()) {
					return 1;
				}else {
					return -1;
				}
			}	
};
Comparator<Employee> secComp = new Comparator<Employee>() {
			@Override
			public int compare(Employee o1, Employee o2) {
				if(o1.getDob().isAfter(o2.getDob())) {
					return 1;
				}else {
					return -1;
				}
			}
};
firstComp.thenComparing(secComp);
Collections.sort(list,firstComp);
list.forEach(a -> System.out.println(a));
		
		
// Using Stream API
list.stream()
.sorted(Comparator.comparing(Employee::getId).thenComparing(Employee::getDob))
.collect(Collectors.toList())
.forEach(a -> System.out.println(a));
```
- Example of list of Non - Object	
```
// Sort list based on length of string
List<String> listStr = new ArrayList<String>();
listStr.add("aaaa");
listStr.add("aa");
listStr.add("aaa");
Comparator<String> cmp = new Comparator<String>() {
	@Override
	public int compare(String o1, String o2) {
		return o1.length() > o2.length() ? 1 : -1;
	}
};
Collections.sort(listStr,cmp);
listStr.forEach(a -> System.out.println(a));
		
// Using Stream	
listStr.stream().sorted(
(o1, o2) -> o1.length() > o2.length() ? 1 : -1
)
.forEach(a -> System.out.println(a));
```



## Comparable: Natural Sorting
- Add default sorting logic in class itself
- implement Comparable<T> and add same logic like Comparator in 'compareTo() method'

```
public class Employee implements Comparable<Employee>{
  int id;
  String name;
	@Override
	public int compareTo(Employee o) {
		if(this.id > o.id) {
			return 1;
		}else {
			return -1;
		}
	}
}
```






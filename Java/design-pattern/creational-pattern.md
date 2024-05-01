## Abstract the processing of Creating an Object and hiding its implementation by exposing the Interface

## 1. Singleton
- Ensuring only one instance is created
```
Public class SingletonClass{
	private static final SingletonClass obj ;
	private SingletonClass(){ }
	public SingletonClass getInstance(){
		if(obj == null){
			obj = new SingletonClass();
		}
		return obj;
	}
}
```

## 2. Factory Pattern
- Its creational Design Pattern provides an Interface for creating Objec without specifying a concrete class. It encapsulates object creational logic in the separate class known as the factory.
- Problem: Currently we have a single Trade Type (SLB). All its creational code is present in its Concrete class (SLBTradeCreator). In order to create SLB Trade we create an instance of SLBTradeCreator using ‘new’. But what if we have another Trade? We need to modify and configure a major part of the code.
- Solution: The Factory Method pattern suggests that you replace direct object construction calls (using the new operator) with calls to a special factory method. So that whenever any new Trade Adds we only need to configure it in Factory Class instead of doing changes in our service class.

```
# Trade Hierarchy 
interface Trade {
	public void execute();
}
public class TradeA implements Trade{
	@Override
	public void execute() {
		System.out.println("Execute Trade A " + this.getClass().getSimpleName());
	}
}
public class TradeB implements Trade{
	@Override
	public void execute() {
		System.out.println("Execute Trade B " + this.getClass().getSimpleName());
	}
}

# Factory
public class TradeFactory {
	public static Trade createTrade(String type) {
		if("TradeA".equals(type)) {
			return new TradeA();	
		}else if("TradeB".equals(type)) {
			return new TradeB();
		}
		return null;
	}
}
# call / Use case
Trade a = TradeFactory.createTrade("TradeA");
a.execute();
		
Trade b = TradeFactory.createTrade("TradeB");
b.execute();
```

## 3. Buidler Pattern
- Ref: https://medium.com/javarevisited/builder-design-pattern-in-java-3b3bfee438d9
- Allows creation of Objects with required fields and hides other fields thus hiding complexity 
- Problem: lots of parameters in construction, in most cases most of the parameters are unused, making the constructor ugly.
```
Class Product{
	public Product(value1,value2,value3,value4,value5){
		// set all values
	}
}
// Product type 1
new Product(value1,value2,value3,null, null);
// Product type 2
new Product(null,value2,value3,value4,value5);
```
- Solution: Extract the Object creation code in its own subclass class called a builders
- allows you to produce different types and representations of an object using the same construction code
- There Is a Problem With This Approach, That Is the Object State Will Be Inconsistent Unless All the Attributes Are Set Explicitly.

```
Final class Product{
	private int id;
	private String name;
	private String description; // Optional
	private String field; // Optional
	
	public Product(ProductBuilder builder){
		this.id = builder.id;
		this.name = builder.name;
		this.description = builder.description;
		this.field = builder.field;
	}	

	// getter for all fields

	public static ProductBuilder{
		private int id;
		private String name;
		private String description; // Optional
		private String field; // Optional

		public ProductBuilder(int id, String name){ // required fields
			this.id = id;
			this.name = name;
		}
		public ProductBuilder setDescription(String desc){ // optional
			this.description = desc;
			return this.
		}
		public ProductBuilder setField(String field){
			this.field = field;
			return this;
		}
		public Product build(){
			return new Product(this);
		}	
	}
}
```


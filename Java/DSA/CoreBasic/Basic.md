
## Formulas for Coding in Java


<details><summary>STD I/O</summary>

```java
Scanner sc = new Scanner(System.in);
// Read integer
int n = sc.nextInt();
// Read string
String str = sc.nextLine();
// Read array of integers
int[] arr = new int[n];
for (int i = 0; i < n; i++) {
    arr[i] = sc.nextInt();
}
// Read array of strings
String[] strArr = new String[n];
for (int i = 0; i < n; i++) {
    strArr[i] = sc.nextLine();
}
// Read 2D array of integers
int[][] matrix = new int[n][m];
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        matrix[i][j] = sc.nextInt();
    }
}
```
</details>


<details>
<summary>Formulas for shapes; Squar, circle, tringle</summary>

```
Circle:
    Area = Math.PI  * r^2
    Circumference (Outer boundry distance)= 2 * Math.PI * r
Square:
    Area = a^2
    Perimeter (Outer boundry distance) = 4 * a
Triangle:
    Area = 0.5 * b * h
    Perimeter = a + b + c
```

</details>


<details>
<summary>1. Find the missing number:  Input: [1,2,3,4,6] , Output: 5 </summary>

#### Find sum of n numbers; Substract sum from array elements;
    - sum = n*(n+1)/2
    - missing = sum - array[i]

#### Using XOR :
		- When compared by bit: for same -> 0, for diff -> 1
		- When comparing numbers or alphabet, it compares bit by bit
			2(0010)^2(0010) -> 0 , 5(0101)^3(0011)->6(0110)
		- actual array (1^2^3^4) ^ provided arry (1^2^4)
			It compares pair, pair get cancels and odd is left
```
public int findMissing(int[] arr, int n) {
    	int xor = 0;
    	for (int i = 1; i <= n; i++) {
       	 xor ^= i;
    	}
    	for (int num : arr) {
       	 xor ^= num;
    	}
   		 return xor;
	}
```
</details>

<details>
<summary>2. Prime number: Number that can have only factor , 1 and itself<br>
</summary>

```
private static boolean isPrime(int number){
        for(int i=2; i<=number/2;i++){
            if(number % i == 0)  return false;
        }
        return true;
    }
```
</details>


<details>
<summary> 3. Two Sum , INPUT: nums = [2, 7, 11, 15], target = 9 ; Output: [0,1] - [2,7]
</summary>

```declarative
Set<Integer> set = new HashSet<>();
//      Map<Integer,Integer> map = new HashMap<>();
      for(int i = 0; i< nums.length; i ++){
          int temp = target - nums[i];
          if(set.contains(temp)){
              System.out.println(nums[i] + " + " + temp + " = " + target);
              break;
          }
          set.add(nums[i]);
//          map.put(nums[i], i);
      }
```


</details>



<details>
<summary>3. Reverse String <br> </summary>

#### Using StringBuilder
```
StringBuilder sb = new StringBuilder(str);
sb.reverse();
```

#### Using Char Array
```
for(int i=str.length()-1;i>=0;i--){
    System.out.print(str.charAt(i));
}
```

#### Using tocharArray
char[] arr = str.toCharArray();
for(int i=str.length() - 1; i >= 0; i--)
    st.append(arr[i]);

#### Using Recursion
```
public static String reverseString(String str) {
    if (str.isEmpty()) {
        return str;
    }
    return reverseString(str.substring(1)) + str.charAt(0);
}
```
</details>
# Map

| HashMap | TreeMap | LinkedHashMap | HashTable | 
|---|---|---|---|
| Random order | sorted by key | insertion order | Random Order |
| Not-Synchronized/NotThreadSafe | Not-Synchronized/NotThreadSafe | Not-Synchronized/NotThreadSafe | Synchronized | 
| Bucket | RedBlackTree | doubly linked list of bucket | Bucket |   
| null allowed | null not-allowed | null allowed | null not-allowed |
| interface:Map | Map, SortedMap, NavigableMap | Map | Map | 

## HashMap
- Bucket[key,value,nextAddress]
- Default size = 16
- Not Synchronized , So not Thread safe. Can be made Thread safe using "Collection.synchronizedMap(HM)"

**Internal Working**
- While inserting data in HashMap, the index is calculated,
  - index = hashCode(key) & (n-1)
  - hashcode of null is '0'
- If two keys have the same index, this is called a collision. In such cases, data is stored as a linked list. After some threshold is reached linked list is converted to bTree
While retrieving data,  fetch data based on the calculated index. Additionally, it also uses equals to get correct data.

## ConcurrentHashMap
- CHM will not lock whole map, Instead it divides Map into segments (Default 16 segment, Here Max 16 Thread can work at time) and locks are applied over them.
- When updating/Setting data to specific segment. Lock will be aquired and released once operation completes.
  - Thus, Multiple operation can work simultaneously
  - Stages
    1. Wait for Lock
    2. Perform Operation
    3. Release Lock

## HashTable : Thread safe/Synchronized . !!!Need more research for how it works!!!
- HashTable uses same Sychronized technique which is used with Collection.synchronized
- Does not allow null key/value
index = sum ( ASCII value of letter of key ) % size of current Map
in case of collision, It will search for another location using LINEAR PROBING 

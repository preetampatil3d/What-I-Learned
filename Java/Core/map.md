# Map


| HashMap | TreeMap | LinkedHashMap | HashTable | 
|---|---|---|---|
| Random order | sorted by key | insertion order | Random Order |
| Not-Synchronized/NotThreadSafe | Not-Synchronized/NotThreadSafe | Not-Synchronized/NotThreadSafe | Synchronized | 
| Bucket | RedBlackTree | doubly linked list of bucket | Bucket |   
| null allowed | null not-allowed | null allowed | null not-allowed |
| interface:Map | Map, SortedMap, NavigableMap | Map | Map |
| **Default Bucket Size** | 16 buckets | N/A (No buckets used) | 16 buckets | 11 buckets |
| **How Size Expands** | Doubles (`capacity * 2`) when size exceeds threshold (`capacity * 0.75`) | Dynamically allocates individual tree nodes on every insert; no resizing overhead | Doubles (`capacity * 2`) when size exceeds threshold (`capacity * 0.75`) | Almost doubles (`capacity * 2 + 1`) when size exceeds threshold (`capacity * 0.75`) |
| **How Elements Store Inside Single Bucket** | Linked List; converts to a Red-Black Tree if a bucket grows past 8 elements (and total capacity ≥ 64) | N/A (Stored directly as self-balancing Red-Black Tree nodes) | Linked List (plus an end-to-end doubly linked list for order); converts to a Red-Black Tree past 8 elements | Strictly as a Linked List (never converts to a tree; prone to performance degradation) |

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

### Conceptual Interview Questions

#### 1. Why should you choose `ConcurrentHashMap` over `Hashtable` in a multi-threaded application?
* **Answer:** **Performance and scalability.** `Hashtable` synchronizes every method using a single lock on the entire object, meaning only one thread can read or write at a time. `ConcurrentHashMap` uses bucket-level locking (lock striping) and non-blocking reads. This allows multiple threads to read and write concurrently to different parts of the map without blocking each other.

#### 2. Both `Hashtable` and `ConcurrentHashMap` are thread-safe. How do their locking mechanisms differ under the hood?
* **Answer:** 
  * **`Hashtable`:** Uses a **global object lock**. Every `get` and `put` operation acquires the lock on the entire table instance.
  * **`ConcurrentHashMap` (Java 8+):** Uses **volatile variables and `synchronized` blocks on individual bucket head nodes**. It only locks the specific bucket array index being written to. If two threads are accessing different buckets, they run completely in parallel. Furthermore, read operations do not require locks at all.

#### 3. Why do neither `Hashtable` nor `ConcurrentHashMap` allow `null` keys or values, while `HashMap` does?
* **Answer:** To avoid **ambiguity in concurrent environments**. If `get(key)` returns `null` in a standard `HashMap`, you can call `containsKey(key)` to determine if the key is missing or if the value is explicitly `null`. In a multi-threaded map, another thread could insert or delete the key between those two calls (a race condition), making the result unreliable. Forcing non-null values eliminates this ambiguity.

#### 4. What is "Lock Striping" or bucket-level locking in `ConcurrentHashMap`?
* **Answer:** It is a technique where instead of protecting a whole collection with one lock, the collection is split into independent parts (buckets), each with its own lock. In `ConcurrentHashMap`, synchronization is applied only to the head node of a bucket when a write operation occurs, minimizing thread contention.

#### 5. What are "Fail-Fast" and "Fail-Safe / Weakly Consistent" iterators?
* **Answer:** 
  * **Fail-Fast (`Hashtable`, `HashMap`):** If the map is structurally modified by a thread while another thread is iterating over it, the iterator immediately throws a `ConcurrentModificationException`.
  * **Fail-Safe / Weakly Consistent (`ConcurrentHashMap`):** The iterator works on a snapshot or view of the data at the time it was created. It will not throw an exception if the map is modified during iteration, and it safely reflects updates made after creation, though it isn't guaranteed to reflect the absolute latest state instantaneously.

#### 6. What are atomic compound actions in `ConcurrentHashMap`, and why are they important?
* **Answer:** Standard operations like checking if a key exists and then inserting it (`if (!map.containsKey(k)) map.put(k, v)`) are not safe across threads, even in a concurrent map, because another thread can slip in between the check and the act. `ConcurrentHashMap` provides atomic compound methods like **`putIfAbsent()`**, **`computeIfAbsent()`**, and **`replace()`** to execute these multi-step operations safely in a single, atomic step without external synchronization.

### Coding & Scenario-Based Interview Questions

#### 1. The "Check-Then-Act" Race Condition
**Scenario:** You are building a cache using `ConcurrentHashMap`. Multiple threads execute the following code to populate it. What is the bug, and how do you fix it?
```java
// Thread-unsafe compound action
if (!cache.containsKey(key)) {
    cache.put(key, expensiveDatabaseCall(key));
}
```
* **The Bug:** It is not atomic. Two threads could simultaneously pass the `containsKey` check, causing `expensiveDatabaseCall` to run twice for the exact same key.
* **The Fix:** Use `computeIfAbsent`, which executes atomically.
```java
cache.computeIfAbsent(key, k -> expensiveDatabaseCall(k));
```

#### 2. Atomic Counter Updates
**Scenario:** You need to count the frequency of words across multiple incoming web requests using a shared map. Why is the code below broken, and how do you fix it?
```java
// Thread-unsafe update
Integer count = wordCounts.get(word);
wordCounts.put(word, count == null ? 1 : count + 1);
```
* **The Bug:** `get` and `put` are individually thread-safe, but combined they create a race condition where updates from other threads get overwritten.
* **The Fix:** Use `merge` or `compute` to perform an atomic read-modify-write operation.
```java
wordCounts.merge(word, 1, Integer::sum);
```

#### 3. Conditional Replacement
**Scenario:** A background thread needs to update a user's status token in a shared map, but only if the status is currently set to `"PENDING"`. How do you do this cleanly without manual synchronization blocks?
* **The Fix:** Use the atomic `replace(K key, V oldValue, V newValue)` method. It returns `true` only if the current value matches the expected `oldValue`.
```java
boolean updated = statusMap.replace(userId, "PENDING", "ACTIVE");
if (updated) {
    // Successfully moved from PENDING to ACTIVE
}
```

#### 4. The `computeIfAbsent` Pitfall
**Scenario:** Inside a `computeIfAbsent` block, a developer tries to modify a completely different key in the exact same `ConcurrentHashMap`. What risk does this introduce?
* **The Bug:** It can cause a **deadlock** or a live lock. The map locks the bucket of the original key while running the lambda function. If the lambda triggers actions that lock other parts of the map, it can stall the runtime engine.
* **The Rule:** Keep mapping functions inside `compute` variants short, pure, and free of side-effects on the same map.





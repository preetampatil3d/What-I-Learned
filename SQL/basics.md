### SQL Tunning 

1. Indexing:  Proper indexing can significantly improve query performance. Identify the columns that are frequently used in WHERE and JOIN clauses and create indexes on those columns.

2. QueryOptimizing by avoiding distinct, subqueries, direct join, wildcard
	- Avoid Distinct: Internally DISTINCT uses GroupBy for all fields to get distinct results.
	Instead, we can use GroupBy directly with more filters to get to correct result if possible.
	- Avoid direct JOIN: with WHERE Instead use INNER JOIN	
		- Using JOIN will create Cartesian Join. Which is to create all possible combinations then the filter is applied. Which will reduce performance in the case of huge tables.
	- Avoid Nested/Sub Query: Instead use INNER  JOIN
	- Avoid wildcard ( < > ) : If a direct keyword can used like ‘BETWEEN’
 - SARGABLE : **S**earch **ARG**gument **ABLE**
   -  For filter in where claus use field data darectly instead of converting and use
   -  Avoid using function for index column, use direct comparisn whenever possible
   -  SARGGABLE: specificDate <= '01-01-2024'
   -  NON-SARGABLE: YEAR(specificDate) <= '2024' (Year function applied on all fields, it will make query slover)
3. Data Normalization: remove redundant data and improve performance.
4. Use stored Proceedures:
5. Use pagination


 ### How does the Index work?
 
- When we apply index for Column, Data Structure/View is created with indexedColumn and pointer to a specific row. 
	- Data structure is sorted by indexed column with ‘BTree/Binary Tree’
	- Pointer is an address in memory of a specific row/Record.
- The query looks for the specific row in the index; the index refers to the pointer which will find the rest of the information.
- Indexing reduces the number of row query has to search



### Using the CASE statement
- Condition-based result. We can use CASE in select or in ORDER BY

```
SELECT id, qty,
	CASE
		WHEN qty > 10 THEN ‘qty is greater than 10’
		WHEN qty = 10 THEN ‘qty is 10’
		ELSE ‘Qty is less than 10’
	END
FROM inventory
```
```
SELECT id, city, country
FROM loc
ORDER BY
	(CASE
		WHEN city == null THEN country
		ELSE city
	END)
```

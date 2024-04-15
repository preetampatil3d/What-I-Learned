## Basic Clause

| Clause | Description/ Usage |
| --- | --- |
| SELECT TOP 'number' | select top number of records |
| LEFT(field,num_of_char) | select num_of_char character from left of field |
| RIGHT(field,num_of_char) | select num_of_char character from right of field |
| UNION | combine two queries, Query1 UNION Query2  |
| GROUP BY | used to find count w.r.t. specific field, Eq: Find number of Employee in each project |
| HAVING | used to filter query with agreegation (min, max). It is used only with GROUP BY  |
| EXISTS  | where EXIST (subquery), return true/false. if subquery returns any data return true else false |
| ANY | used with where, WHERE projectId = ANY (subquery), any matches in table will return true |
| ALL | used with SELECT, WHERE, HAVING, return true only if it matches all records in table  |
| INTO | Copy data from another table, select * INTO newTable from existingTable |
| INTO IN | Copy data from external table, select * INTO newTable in 'data.mdb' from existingTable |
| INSERT INTO SELECT | Copy all columns from one table to another |
| IFNULL(field, 'defaultValue') | function let declare default value if value is doesn't exist | 

## Operator

| Operator	| Description	Example |
| --- | --- |
| ALL |	TRUE if all of the subquery values meet the condition	|
| AND |	TRUE if all the conditions separated by AND is TRUE	|
| ANY |	TRUE if any of the subquery values meet the condition |	
| BETWEEN |	TRUE if the operand is within the range of comparisons |	
| EXISTS |	TRUE if the subquery returns one or more records |
| IN | TRUE if the operand is equal to one of a list of expressions |	
| LIKE | TRUE if the operand matches a pattern |
| NOT	| Displays a record if the condition(s) is NOT TRUE |	
| OR |	TRUE if any of the conditions separated by OR is TRUE |	
| SOME | TRUE if any of the subquery values meet the condition |
| SQRT(number) | |


### ANY
- returns boolean values. If any result matches from subquery it returns true.
- Used with WHERE clause
```
select projectName from PROJECT
where projectId = ANY (select projectId from PROJECT where category = 'finance')
```

### ALL
- returns boolean values. If all result matches from subquery it returns true.
- Used with SELECT, WHERE, HAVING clause
- If we consider project table have many categories, then Below query will return false.
- It will return true only if we have only one category 'finance'
```
select projectName from PROJECT
where projectId = ALL (select projectId from PROJECT where category = 'finance')
```

### INSERT INTO SELECT
- Copy all columns from one table and insert into another table
```
INSERT INTO user (name, city)
SELECT supplierName, city from supplier
```



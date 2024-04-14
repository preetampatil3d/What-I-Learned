# Joins
### Sample Tables
EmployeeTable
| id | name | deptId |
|---|---|---|
| 1 |  Ename1 | 1 |
| 2 |  Ename2 | 2 |
| 3 |  Ename3 | 2 |
| 4 |  Ename4 | 5 |
| 5 |  Ename5 | null |

DepartmentTable
| id | name |
|---|---|
| 1 |  Ename1 |
| 2 |  Ename2 |
| 3 |  Ename3 |

## INNER JOIN / JOIN
- Returns only those records which match the condition. Or Records that have matching value in both tables.
- Ex: it will return Employees whose deptId Is present in the department table
```
# 
Select * from employee e 
Join department d on d.id = e.deptId; // JOIN is same as INNER JOIN

#
Select * from employee e, department d
Where d.id = e.deptId

```


## LEFT OUTER JOIN / LEFT JOIN
- Return all records from the left table and only those records from the right table which match join condition.
- Ex: All Employees + Employees whose deptId is present in the department, deptId is set to null whose department is not found.


## RIGHT OUTER JOIN / RIGHT JOIN
- Return all records from the right table and only those records from the left table which match join condition.
- Ex: All Department + Employee whose deptId is present in Department, ename is set to null for record of dept which not linked to any Employee.

## FULL OUTER JOIN :  Not support in Mysql
- Return all records, It is a combination of both left and right join which contains records from both tables
- Ex: all records from both tables, Set values to null if no respective reference is found
- Not supported in Mysql, But we can achieve it using UNION
```
Select * from employee e 
Left Join department d on d.id = e.deptId; 
UNION
Select * from employee e 
Right Join department d on d.id = e.deptId;

```

## SELF JOIN
- Used to join table with self
```
# Select employees whose department is the same.
Select * from employee e1, employee e2 
Where e1.depatId = e2.deptId AND e1.id != e2.id
```

## CROSS JOIN
- when each row from the first table is combined with each row from the second table, It is called as cartesian JOIN or CROSS JOIN
```
Select * from Employee , Department

Select * from Employee CROSS JOIN Department

```

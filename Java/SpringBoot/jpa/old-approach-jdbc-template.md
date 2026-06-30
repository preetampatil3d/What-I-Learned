# Spring JDBC Notes

## 1. JdbcTemplate

### What is it?

`JdbcTemplate` is a Spring utility class that simplifies JDBC operations
by handling: - Connection management - Statement creation - Exception
handling - Resource cleanup

### When to use

-   Simple SQL queries
-   Small number of parameters (1--3)
-   Fixed SQL statements

### Uses positional parameters (`?`)

``` java
String sql = "SELECT * FROM employee WHERE id = ?";

Employee emp = jdbcTemplate.queryForObject(
    sql,
    new EmployeeRowMapper(),
    empId
);
```

### Insert Example

``` java
String sql = "INSERT INTO employee(id, name) VALUES(?, ?)";

jdbcTemplate.update(sql, id, name);
```

### Advantages

-   Less boilerplate
-   Easy to learn
-   Good for simple queries

### Limitations

-   Parameter order matters
-   Difficult to maintain with many parameters

------------------------------------------------------------------------

## 2. NamedParameterJdbcTemplate

### What is it?

`NamedParameterJdbcTemplate` extends `JdbcTemplate` and supports **named
parameters** instead of positional parameters.

### When to use

-   Queries with multiple parameters
-   Dynamic SQL
-   Enterprise applications
-   Better readability and maintainability

### Uses named parameters (`:parameterName`)

``` java
String sql = "SELECT * FROM employee WHERE department = :dept AND status = :status";

MapSqlParameterSource params = new MapSqlParameterSource()
        .addValue("dept", department)
        .addValue("status", status);

List<Employee> employees =
        namedParameterJdbcTemplate.query(
                sql,
                params,
                new EmployeeRowMapper());
```

### Insert Example

``` java
String sql = "INSERT INTO employee(id, name) VALUES(:id, :name)";

MapSqlParameterSource params = new MapSqlParameterSource()
        .addValue("id", id)
        .addValue("name", name);

namedParameterJdbcTemplate.update(sql, params);
```

### Advantages

-   Readable SQL
-   Parameter order doesn't matter
-   Easy to build dynamic queries
-   Easier maintenance

### Limitations

-   Slightly more code for simple queries

------------------------------------------------------------------------

## RowMapper Example

``` java
public class EmployeeRowMapper implements RowMapper<Employee> {

    @Override
    public Employee mapRow(ResultSet rs, int rowNum)
            throws SQLException {

        return new Employee(
                rs.getLong("id"),
                rs.getString("name"),
                rs.getString("department")
        );
    }
}
```

------------------------------------------------------------------------

## Quick Comparison

  Feature                   JdbcTemplate     NamedParameterJdbcTemplate
  ------------------------- ---------------- ----------------------------
  Parameters                `?`              `:name`
  Best for                  Simple queries   Complex & dynamic queries
  Readability               Medium           High
  Parameter order matters   Yes              No
  Dynamic SQL               Difficult        Easy

------------------------------------------------------------------------

## Interview Summary

> **JdbcTemplate** is ideal for simple, fixed SQL queries using
> positional (`?`) parameters.
>
> **NamedParameterJdbcTemplate** is preferred for enterprise
> applications because named parameters improve readability, eliminate
> parameter-order mistakes, and simplify dynamic SQL.

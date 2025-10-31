### PrimaryKey vs Unique Key
| Primary Key | Unique Key |
| --- | ---|
| Primary key is field or combination of field which uniquely identify record | Unique key is field which is unique data in column |
| Can not be null | Can be null, By default null be treated as distinct. |
| | Specifying 'NULLS NOT DISTINCT' will not treat null as distinct |
| One Primary key per table | can have multiple unique columns |

### Foreign Key
- Foreign key is field in one table that referes to primary key in another table

### Compound Key: Combination of foreign keys
- Combination of multiple foreign keys present in same table to achieve uniqueness for the record.

### Composite Key: Combination of any fields
- Combination of multiple columns (any) in same table to achieve uniqueness for the record.

### Create Compound or Composite key using @IdClass or @EmbededId
- Used to create a composite key, which is a combination of columns to form a primary key.

- Using @EmbededId
```
@Embeddable
public class BookId implements Serializable {
    private String title;
    private String language;

    // default constructor

    public BookId(String title, String language) {
        this.title = title;
        this.language = language;
    }
    // getters, equals() and hashCode() methods
}

@Entity
public class Book {
    @EmbeddedId
    private BookId bookId;
    // constructors, other fields, getters and setters
}


```

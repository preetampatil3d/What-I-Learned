
## @IdClass & @EmbededId
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


# Database Pagination: OFFSET vs. Cursor Pagination

## 📝 Definition
**`OFFSET`** specifies the exact number of initial rows to skip before the database engine starts returning results from a query.

---

## ⚠️ The Performance Problem

### The Scenario
You have a table with millions of rows. Using a high offset (like `OFFSET 500000`) forces the database to read, sort, and discard half a million rows in memory just to return the next 10 results. This causes severe query lag and high resource consumption.

### The Solution: Cursor Pagination
Instead of counting rows to skip, you store the unique identifier (like an `ID` or `Timestamp`) of the **very last item** on the current page. The next query simply filters for records that come immediately after that value.

---

## 💻 SQL Comparison

### Avoid This (Offset-Based)
Slows down drastically as the page number increases.
```sql
-- Fetching Page 51 (Skipping 500 rows)
SELECT id, title, created_at 
FROM posts 
ORDER BY id ASC 
LIMIT 10 OFFSET 500;
```

### Use This (Cursor-Based / Seek Method)
Execution speed remains instant and constant regardless of the page number.
```sql
-- Step 1: Fetch Page 1 and record the last ID seen (e.g., ID is 4582)
SELECT id, title, created_at 
FROM posts 
ORDER BY id ASC 
LIMIT 10;

-- Step 2: Fetch Page 2 using the last ID as a benchmark instead of OFFSET
SELECT id, title, created_at 
FROM posts 
WHERE id > 4582 
ORDER BY id ASC 
LIMIT 10;
```

---

## 📊 Summary Comparison

| Feature | Offset Pagination | Cursor Pagination |
| :--- | :--- | :--- |
| **Performance** | Destroys database performance on deep pages | Remains fast across millions of rows |
| **Data Drift** | Skips items or shows duplicates if rows are added/deleted | Handles real-time data insertions smoothly |
| **Use Case** | Best for exact page navigation (e.g., jump to page 14) | Best for infinite scroll feeds (e.g., social media timelines) |

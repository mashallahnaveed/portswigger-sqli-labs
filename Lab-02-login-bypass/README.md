# Lab 02 – SQL Injection Vulnerability Allowing Login Bypass

## Lab Information

- **Platform:** PortSwigger Web Security Academy
- **Category:** SQL Injection
- **Difficulty:** Apprentice
- **Status:** ✅ Solved

---

## Objective

The goal of this lab is to exploit an SQL Injection vulnerability in the login functionality and log in as the **administrator** user without knowing the password.

---

## Vulnerability

The application does not properly sanitize user input before including it in an SQL query.

Because of this, SQL code can be injected into the **username** field to manipulate the login query and bypass authentication.

---

## Payload Used

```sql
administrator' --
```

---

## How It Works

Normally, the application executes a query similar to:

```sql
SELECT * FROM users
WHERE username = 'administrator'
AND password = 'your_password';
```

After injecting:

```sql
administrator' --
```

the query becomes:

```sql
SELECT * FROM users
WHERE username = 'administrator' --'
AND password = 'your_password';
```

The `--` starts a SQL comment, so everything after it is ignored.

As a result:

- The password check is ignored.
- The application only verifies the username.
- Authentication succeeds if the username exists.

---

## Steps Performed

### 1. Open the Lab

Read the lab description and understand the objective.

![Lab Details](images-02/1.%20lab%20details.png)

---

### 2. Visit the Login Page

Navigate to **My Account** to access the login form.

![Homepage](images-02/2.%20homepage.png)

---

### 3. Inject the Payload

Enter the following payload into the **Username** field:

```sql
administrator' --
```

Enter **any value** (for example, `hello`) in the password field and click **Log in**.

![Bypassing Login](images-02/3.%20bypassing.png)

---

### 4. Successful Login

The application logs in as the **administrator**, confirming that the SQL Injection attack successfully bypassed authentication.

![Lab Solved](images-02/4.%20result.png)

---

## Key Learning Points

- SQL Injection can affect login forms.
- Authentication can be bypassed when user input is not sanitized.
- The `--` operator comments out the rest of the SQL query.
- Parameterized queries help prevent SQL Injection attacks.
- User input should never be directly concatenated into SQL statements.

---

## Mitigation

Developers should:

- Use parameterized (prepared) SQL statements.
- Validate and sanitize user input.
- Never concatenate user input directly into SQL queries.
- Apply the principle of least privilege to database accounts.

---

## What I Learned

- How SQL Injection can bypass authentication.
- How SQL comments (`--`) change the execution of a query.
- Why using prepared statements is important for preventing SQL Injection.
- How a small input validation mistake can lead to a serious security vulnerability.

---

## Lab Status

**Completed Successfully** ✅

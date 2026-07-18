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
AND password = 'password';
```

After injecting:

```sql
administrator' --
```

the query becomes:

```sql
SELECT * FROM users
WHERE username = 'administrator' --'
AND password = 'anything';
```

The `--` starts a SQL comment, so everything after it is ignored.

As a result:

- The password check is removed.
- The application only checks whether the username is **administrator**.
- Login succeeds without knowing the password.

---

## Steps Performed

### 1. Open the Lab

Access the login page.

![Lab Details](images/step1-lab-details.png)

---

### 2. Visit the Login Form

Navigate to **My Account** to reach the login page.

![Login Page](images/step2-login-page.png)

---

### 3. Inject the Payload

Enter the following into the **Username** field:

```sql
administrator' --
```

Enter any value in the password field and click **Log in**.

![Payload](images/step3-login-bypass.png)

---

### 4. Successful Login

The application logs in as the **administrator**, confirming that the SQL Injection attack successfully bypassed authentication.

![Lab Solved](images/step4-lab-solved.png)

---

# Key Learning Points

- SQL Injection can affect login forms.
- Authentication can be bypassed when user input is not sanitized.
- `--` is used to comment out the remaining SQL query.
- Input validation and parameterized queries help prevent SQL Injection.

---

## Mitigation

Developers should:

- Use parameterized (prepared) SQL statements.
- Validate and sanitize user input.
- Never concatenate user input directly into SQL queries.
- Apply least-privilege access to database accounts.

---

## Lab Status

**Completed Successfully** ✅

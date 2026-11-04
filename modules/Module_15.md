**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 15 — SQL Injection**

*Beginner-friendly · Scenario-ready · What to memorize cold · HIGH-ROI module*

Sources: Project documents (15.1–15.5 study sheets) + Google Drive (CEH V13 Master Study Guide). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

**SQL injection (SQLi)** is an attack that exploits **UNSANITIZED** user input to run malicious SQL queries against a database. The attacker's text is treated as SQL code instead of data.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The form-field picture</strong></p>
<p><strong>•</strong> A login form expects a username. The app glues your input straight into a database query.</p>
<p><strong>•</strong> If you type SQL instead of a name, the database runs it — because the input was never separated from the code.</p>
<p><strong>•</strong> Core sentence: SQL injection happens because user input is CONCATENATED into SQL queries without validation.</p></td>
</tr>
</tbody>
</table>

# **2) How it works (the classic example)**

Normal login query the app builds:

SELECT \* FROM Users WHERE username='smith' AND password='simpson';

Attacker types blah' OR '1'='1 as the username. The query becomes:

SELECT \* FROM Users WHERE username='blah' OR '1'='1' AND password='anything';

- **Why it works:** OR '1'='1' is ALWAYS true, so the WHERE clause is always true → authentication bypassed.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Memory hook</strong></p>
<p><strong>•</strong> Flow: Input → Query → Execute → Control. 'True condition breaks logic.'</p>
<p><strong>•</strong> Language (PHP, .NET, Java) is irrelevant — the DATABASE is the target.</p></td>
</tr>
</tbody>
</table>

# **3) Where it occurs & why it's dangerous**

| **Where SQLi occurs** | **What an attacker can do**             |
|-----------------------|-----------------------------------------|
| Login forms           | **Bypass** authentication               |
| Search fields         | **Extract** (read) database data        |
| URL parameters (GET)  | **Modify** or **delete** records        |
| Cookies               | **Execute** admin operations            |
| HTTP headers          | **Remote** code execution (OS commands) |

- **Impact order:** Bypass → Read → Modify → Delete → Execute. Databases targeted: MySQL, MSSQL, Oracle, PostgreSQL, SQLite.

- Where? Forms, URLs, cookies, headers.

- Impact? Bypass, Read, Modify, Delete, Execute.

# **4) Types of SQL injection (the \#1 exam structure)**

| **Category**        | Type                      | Story example                                                                         | Technical explanation                                                                                          | How the attacker does it                                                                                                       |
|---------------------|---------------------------|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|
| In-band             | Error-based               | The receptionist makes a mistake and accidentally reveals secret information.         | Database errors reveal information about the database structure.                                               | Sends unexpected input that triggers a database error, then examines the error message.                                        |
| In-band             | UNION-based               | The receptionist combines two lists and gives you the results together.               | UNION SELECT combines results from another query with the original query's results.                            | Injects a UNION SELECT query into a vulnerable input field to try to retrieve additional data.                                 |
| Inferential (Blind) | **Boolean-based (Blind)** | The receptionist only answers yes or no, so you ask questions to discover the answer. | Infers information by observing differences between TRUE and FALSE responses.                                  | Sends conditions that evaluate to TRUE or FALSE and compares the webpage responses.                                            |
| Inferential (Blind) | **Time-based (Blind)**    | The receptionist doesn't answer directly, so you check how long they take to respond. | Infers information from delays in the application's response.                                                  | Sends a condition that may cause a database delay, such as a database-specific SLEEP function, and measures the response time. |
| Out-of-band         | Out-of-band               | The receptionist sends a message through a different communication channel.           | Retrieves information through a separate channel, such as DNS or HTTP, instead of the normal webpage response. | Attempts to trigger a database-generated DNS or HTTP request to an external system under the attacker's control.               |

| ategory                 | Simple meaning                                                                      | How to remember              |
|-------------------------|-------------------------------------------------------------------------------------|------------------------------|
| **In-band**             | The attacker gets the answer through the same channel (the website).                | Same channel = Direct answer |
| **Blind (Inferential)** | The website doesn't show the answer directly, so the attacker infers it from clues. | No visible output = Infer    |
| **Out-of-band**         | The attacker gets information through a different channel.                          | Different channel = Outside  |

## **Type comparison (exam gold)**

| **Type**         | **Speed** | **What you observe**              |
|------------------|-----------|-----------------------------------|
| Error-based      | **Fast**  | Database error messages           |
| UNION-based      | **Fast**  | Query results on the page         |
| Boolean blind    | Slow      | Page content changes (true/false) |
| Time-based blind | Very slow | Response delay                    |
| Out-of-band      | Medium    | External (DNS/HTTP) callback      |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Memory hook</strong></p>
<p><strong>•</strong> 'In-band → Blind → Out-of-band.'</p>
<p><strong>•</strong> In-band = same channel (fast). Blind = no visible output, you INFER. Out-of-band = different channel.</p></td>
</tr>
</tbody>
</table>

# **5) Blind SQLi payloads (know these cold)**

## **Boolean-based**

' AND 1=1-- (page loads normally = TRUE)

' AND 1=2-- (page differs = FALSE)

**Imagine asking a website a question, but it never shows you the database answer. You compare how the page behaves.**

| **Test**  | **Payload**     | **Simple meaning**                                 |
|-------------------------------------|-------------------------------------------|------------------------------------------------------------------------------|
| **TRUE**  | **' AND 1=1--** | **1 equals 1, so the condition is TRUE.**          |
| **FALSE** | **' AND 1=2--** | **1 does not equal 2, so the condition is FALSE.** |

**Story: You ask two questions. One is always true, and the other is always false. If the page behaves differently, that difference may reveal information.**

## **Time-based (DB-specific delay functions)**

| **Database** | **Delay function** |
|--------------|--------------------|
| MySQL        | SLEEP()            |
| MSSQL        | WAITFOR DELAY      |
| PostgreSQL   | pg_sleep()         |
| Oracle       | DBMS_LOCK.SLEEP    |

- **Rule:** if the response is delayed, the injected condition was TRUE. 'Delay = TRUE.'

<!-- -->

- **The attacker deliberately creates a delay to distinguish between TRUE and FALSE when the website doesn't show the answer directly.**

**Imagine the attacker uses a delay condition:**

- **If TRUE: the database executes SLEEP(5) → the attacker receives the response about 5 seconds later.**

- **If FALSE: the database skips the delay → the attacker receives the response normally, almost immediately**.

# **6) SQL injection methodology (7 phases)**

| Phase                     | Simple explanation                                                                            | Remember for CEH                                   |
|---------------------------|-----------------------------------------------------------------------------------------------|----------------------------------------------------|
| 1\. Detect SQLi           | Check whether website input is vulnerable to SQL injection.                                   | Error, page change, or unusual delay can be clues. |
| 2\. Identify DB           | Discover which database technology the website uses.                                          | MySQL, MSSQL, PostgreSQL, Oracle                   |
| 3\. Enumerate structure   | Discover the database's tables and columns.                                                   | information_schema = database structure            |
| 4\. Extract data          | Retrieve information from the database.                                                       | Usernames, emails, password hashes                 |
| 5\. Bypass authentication | Try to get access without valid credentials.                                                  | Authentication bypass                              |
| 6\. Execute OS commands   | In certain configurations, use the database to run commands on the server's operating system. | xp_cmdshell = MSSQL                                |
| 7\. Maintain access       | Try to keep access after the initial compromise.                                              | Persistence, backdoors, web shells                 |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Memory hook + key objects</strong></p>
<p><strong>•</strong> 'Detect → Identify → Enumerate → Extract → Bypass → Execute → Persist.'</p>
<p><strong>•</strong> information_schema = the metadata database that stores the structure (tables/columns). 'Schema stores structure.'</p>
<p><strong>•</strong> xp_cmdshell (MSSQL) is the classic DB-to-OS bridge.</p></td>
</tr>
</tbody>
</table>

# **7) WAF / filter evasion techniques**

**An attacker tries to change the way an attack is written so the WAF does not recognize it.**

| Technique               | Simple idea                               | Example                                    |
|-------------------------|-------------------------------------------|--------------------------------------------|
| Encoding                | Change how characters are represented     | ' → %27                                    |
| Case manipulation       | Change uppercase/lowercase                | SELECT → SeLeCt                            |
| Comment injection       | Insert SQL comments into a query          | SELECT/\*\*/FROM                           |
| Whitespace manipulation | Replace or modify spaces                  | SELECT/\*\*/FROM                           |
| Operator substitution   | Use an alternative operator               | AND → && (DB-dependent)                    |
| Logical obfuscation     | Rewrite the same logical condition        | 1=1 → 2-1=1                                |
| CHAR()/ASCII            | Construct characters using numeric values | CHAR(65,66,67) → ABC in many SQL databases |
| Concatenation           | Split and join strings or keywords        | Split a keyword into parts                 |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Exam idea</strong></p>
<p><strong>•</strong> 'Blocked ≠ Secure.' A WAF can be bypassed with these tricks, so it is not a silver bullet.</p></td>
</tr>
</tbody>
</table>

# **8) Tools**

**These tools help security testers detect and test SQL injection (SQLi) vulnerabilities in web applications.**

| **Tool**       | **Purpose**                                                        | **Free or Paid?**                                       | **Exam memory**           |
|----------------|--------------------------------------------------------------------|---------------------------------------------------------|---------------------------|
| SQLmap         | Automated SQLi detection, database enumeration and data extraction | Free — open source                                      | **Automated SQLi**        |
| Havij          | GUI-based automated SQLi                                           | Paid / proprietary; availability of old versions varies | **GUI**                   |
| jSQL Injection | Java-based, cross-platform SQLi tool                               | Free — open source                                      | **Java / cross-platform** |
| SQLninja       | SQL injection testing focused on Microsoft SQL Server (MSSQL)      | Free — open source                                      | **MSSQL**                 |
| BBQSQL         | Framework for blind SQL injection testing                          | Free — open source                                      | **Blind SQLi**            |

## **SQLmap options (exam recognition)**

**SQLmap options tell the tool what information to identify or test during an SQL injection assessment.**

sqlmap -u "http://site/page?id=1" **--dbs** \# list databases

sqlmap -u "..." -D shop **--tables** \# list tables

sqlmap -u "..." -D shop -T users **--columns** \# list columns

sqlmap -u "..." -D shop -T users **--dump** \# dump data

sqlmap -u "..." **--os-shell** \# OS shell

# **9) Ports / protocols**

| **Port** | **Database / service** | **Note**                                | **Note to remember**               |
|----------|------------------------|-----------------------------------------|--------------------------------------------------------------|
| 80 / 443 | HTTP/HTTPS             | SQLi travels in web requests (GET/POST) | Web traffic; SQLi payloads can be sent in web requests       |
| 3306     | MySQL                  | Common backend DB                       | Encrypted web traffic; SQLi can still occur                  |
| 1433     | MSSQL                  | xp_cmdshell for OS commands             | Common backend database                                      |
| 1521     | Oracle                 | Enterprise DB                           | xp_cmdshell can execute OS commands if enabled and permitted |
| 5432     | PostgreSQL             | pg_sleep for time-based                 | Common enterprise database port                              |

# **10) Practical example (scenario style)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Scenario walk-through</strong></p>
<p><strong>•</strong> A login page is tested with admin'-- in the username field.</p>
<p><strong>•</strong> The -- comments out the password check, so the query logs the attacker in as admin (auth bypass).</p>
<p><strong>•</strong> Next, the tester finds a search box returns DB errors → error-based SQLi → fingerprints MySQL via @@version.</p>
<p><strong>•</strong> Then UNION SELECT pulls password hashes from information_schema tables.</p>
<p><strong>•</strong> Fix: parameterized queries, input validation, least-privilege DB user, hide errors, WAF.</p></td>
</tr>
</tbody>
</table>

# **11) Countermeasures (prevention checklist)**

<table>
<colgroup>
<col style="width: 6%" />
<col style="width: 34%" />
<col style="width: 59%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>#</strong></th>
<th><strong>Control</strong></th>
<th><strong>Why</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>1</td>
<td>Parameterized queries</td>
<td>Separates code from data — the #1 fix</td>
</tr>
<tr class="even">
<td>2</td>
<td><strong>Prepared statements</strong></td>
<td>Same idea; input can't become SQL</td>
</tr>
<tr class="odd">
<td>3</td>
<td>Input validation</td>
<td>Reject unexpected characters</td>
</tr>
<tr class="even">
<td>4</td>
<td>Least privilege</td>
<td><p>No admin DB user → less damage</p>
<p><mark>Give each user or application only the permissions it needs to do its job.</mark></p></td>
</tr>
<tr class="odd">
<td>5</td>
<td>Hide error messages</td>
<td>Stops error-based info leaks</td>
</tr>
<tr class="even">
<td>6</td>
<td>Patch DBMS</td>
<td>Close known vulns</td>
</tr>
<tr class="odd">
<td>7</td>
<td>Deploy WAF</td>
<td>Blocks common payloads (not a silver bullet)</td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Exam trap</strong></p>
<p><strong>•</strong> Escaping input alone is NOT sufficient. Parameterized queries / prepared statements are the real fix. 'Escape helps, not enough.'</p></td>
</tr>
</tbody>
</table>

# **12) Related concepts**

| **Concept**         | **One-line meaning**                                                                  |
|---------------------|---------------------------------------------------------------------------------------|
| SQLi vs XSS         | SQLi hits the DATABASE; XSS runs script in the BROWSER                                |
| In-band vs Blind    | Output visible vs you must infer it (Vous devez le déduire) |
| information_schema  | Metadata DB holding tables/columns                                                    |
| Parameterized query | Query where input is bound as data, never code                                        |
| xp_cmdshell         | MSSQL feature that runs OS commands                                                   |

# **13) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> Root cause of SQLi = input concatenated into queries without validation (A03 Injection in OWASP).</p>
<p><strong>•</strong> ' OR '1'='1 = authentication bypass (always-true condition).</p>
<p><strong>•</strong> Blind SQLi: Boolean = page changes; Time-based = response delay (SLEEP/WAITFOR).</p>
<p><strong>•</strong> UNION-based needs the SAME number of columns and compatible data types.</p>
<p><strong>•</strong> information_schema is where tables/columns are enumerated.</p>
<p><strong>•</strong> The #1 fix is parameterized queries / prepared statements — NOT just escaping or a WAF.</p>
<p><strong>•</strong> SQL comment symbols: -- (most DBs), # (MySQL), /* */ (all).</p></td>
</tr>
</tbody>
</table>

# **14) Mnemonics**

- **Objectives:** 'Concept → Types → Method → Evasion → Defense → Tools.'

- **Types:** 'In-band → Blind → Out-of-band.'

- **Methodology:** 'Detect, Identify, Enumerate, Extract, Bypass, Execute, Persist.'

- **Core:** 'True condition breaks logic' · 'Delay = TRUE' · 'Schema stores structure.'

- **Defense:** 'Parameterize, don't concatenate.'

# **15) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> SQLi = unsanitized input run as SQL. Cause: input concatenated into queries. OWASP A03 Injection.</p>
<p><strong>•</strong> Classic bypass: ' OR '1'='1 (always true) and admin'-- (comment out password).</p>
<p><strong>•</strong> Types: In-band (Error-based, UNION-based) · Blind (Boolean, Time-based) · Out-of-band (DNS/HTTP).</p>
<p><strong>•</strong> <mark>UNION needs equal column count + compatible types.</mark></p>
<p><strong>•</strong> Time-based delays: MySQL SLEEP(), MSSQL WAITFOR DELAY, PostgreSQL pg_sleep(), Oracle DBMS_LOCK.SLEEP.</p>
<p><strong>•</strong> DB fingerprint: <mark>@@version</mark> (MySQL/MSSQL), <mark>version()</mark> (PostgreSQL).</p>
<p><strong>•</strong> Enumerate via information_schema.tables / .columns.</p>
<p><strong>•</strong> OS commands: MSSQL xp_cmdshell, MySQL INTO OUTFILE.</p>
<p><strong>•</strong> Evasion: encoding, case, comments, whitespace, operator swap, obfuscation, CHAR().</p>
<p><strong>•</strong> Tools: SQLmap (-u, --dbs, --tables, --columns, --dump, --os-shell), Havij, SQLninja, BBQSQL.</p>
<p><strong>•</strong> Fix (in order): parameterized queries, prepared statements, input validation, least privilege, hide errors, patch, WAF.</p>
<p><strong>•</strong> Comments: -- (most), # (MySQL), /* */ (all).</p></td>
</tr>
</tbody>
</table>

*End of Module 15 — SQL Injection. Next up: quiz this module, or move to Module 16 (Hacking Wireless Networks).*

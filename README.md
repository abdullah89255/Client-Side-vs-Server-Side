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



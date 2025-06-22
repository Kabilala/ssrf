
# README — Basic SSRF Against Another Back-End System

---

## 🧠 Overview

This lab demonstrates a **basic SSRF vulnerability** where the application fetches stock data from an internal system. The goal is to exploit this SSRF to scan an internal IP range (`192.168.0.X` on port `8080`), find an admin panel, and perform a malicious action (delete a user).

---

## 🎯 Objective

* Use SSRF to scan the internal network on `192.168.0.1`–`192.168.0.255:8080`
* Identify the `/admin` interface
* Send a crafted SSRF request to `/admin/delete?username=carlos` to solve the lab

---

## 🛠️ Tools

* **Burp Suite Community Edition**

  * Proxy
  * Intruder
  * Repeater

---

## 🔍 Step-by-Step Exploitation

### 1. 🔗 Intercept the Stock Check Request

1. Visit any product page
2. Click on **Check stock**
3. Intercept the request using Burp Suite:

   ```http
   POST /product/stock HTTP/2
   Host: <your-lab-id>.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded

   stockApi=http://stock.weliketoshop.net/product/stock
   ```

---

### 2. 🚀 Launch Burp Intruder to Scan Internal IPs

1. Send the request to **Burp Intruder**
2. Replace the `stockApi` parameter with:

   ```
   http://192.168.0.1:8080/admin
   ```
3. Highlight only the last octet: `1` → and click **"Add §"**



4. Go to **Payloads tab**, configure:

   * Payload type: `Numbers`
   * From: `1`
   * To: `255`
   * Step: `1`



5. Start the attack.

---

### 3. 🎯 Identify the Live Host

* After scan completes, sort by **Status** or **Length**
* One of them will return `200 OK` (the rest may be `403`, `404`, etc.)

📸 *Placeholder:* `./screenshots/host_found.png`

---

### 4. 🧨 Access the Delete Function

1. Send the successful request to **Repeater**
2. Modify the path in `stockApi` to:

   ```
   http://192.168.0.X:8080/admin/delete?username=carlos
   ```
3. Send the request

📸 *Placeholder:* `https://github.com/Kabilala/ssrf/blob/main/lab2/lab2.png`

If successful, the lab is marked as **Solved ✅**

---

## 📊 Summary

| Element          | Value                              |
| ---------------- | ---------------------------------- |
| Type             | SSRF (Server-Side Request Forgery) |
| Scope            | Internal network scanning          |
| Target IP Range  | 192.168.0.1 to 192.168.0.255       |
| Port             | 8080                               |
| Vulnerable Param | `stockApi`                         |
| Final Payload    | `/admin/delete?username=carlos`    |

---

## 🔒 Security Lessons

* SSRF can be used to **interact with private/internal networks**
* Applications should never fetch URLs directly from user input
* Even “harmless” features like stock checkers can be abused
* Whitelisting and strict input validation is **mandatory**

---

## 🛡️ Mitigation Tips

* Block requests to private/internal IPs (`127.0.0.1`, `192.168.*.*`, `10.*.*.*`)
* Use allowlists for approved domains only
* Avoid passing full URLs from user-controlled input
* Monitor DNS + outbound HTTP requests for anomalies

---

## 📁 Suggested Folder Structure

```


---

## ✅ Conclusion

This lab demonstrates how **basic SSRF** vulnerabilities can lead to **internal service discovery** and **unauthorized actions**, such as account deletion — proving why SSRF is a **critical web vulnerability** in real-world applications and cloud environments.

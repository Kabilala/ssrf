Absolutely, Kaouthar! Here's your professional and detailed **README** for:

🎯 **Lab: Blind SSRF with Out-of-Band Detection**

This lab teaches you how to identify **blind SSRF** vulnerabilities using **Burp Collaborator**—a powerful real-world technique in black-box pentesting and bug bounty.

---

# README — Blind SSRF with Out-of-Band Detection (OAST)

---

## 🔍 Overview

This lab demonstrates a **blind Server-Side Request Forgery (SSRF)** vulnerability. Unlike typical SSRF, the vulnerable server **does not return a direct response**, making detection harder.

Instead, the app's **analytics system** fetches the value of the `Referer` header via an internal server-side HTTP request. This makes it a perfect candidate for **out-of-band detection (OAST)**.

---

## 🎯 Objective

* Inject a Burp Collaborator payload into the `Referer` header.
* Detect the **server-side request** to your external domain.
* Confirm the vulnerability via **DNS or HTTP logs** in Collaborator.
* ✅ Mark the lab as solved once interaction occurs.

---

## 🧠 Lab Behavior

* When you visit a product page, the application’s **analytics service** fetches the URL in the `Referer` header.
* This fetch happens **asynchronously** and **server-side**.
* By injecting an external domain, you can detect this behavior even without a direct SSRF response.

---

## 🛠️ Tools

* **Burp Suite** (Community or Pro)

  * Proxy
  * Repeater
  * Collaborator client (built into Burp)

---

## 🚀 Exploitation Steps

### 1. 🎯 Intercept a Product Page Request

* Visit any product in the browser.
* Intercept the **GET** request in Burp Proxy:

```http
GET /product?productId=1 HTTP/2
Host: <your-lab>.web-security-academy.net
Referer: https://<your-lab>.web-security-academy.net/
...
```

---

### 2. ✏️ Modify the Referer Header

* Send the request to **Burp Repeater**.
* In Repeater, **right-click the `Referer` header** and select:

  ```
  Insert Collaborator payload
  ```

This will generate a unique payload like:

```http
Referer: http://xxyz.oastify.com/
```

---

### 3. 📡 Send the Request

* Click **Send** in Repeater.
* Go to the **Burp Collaborator tab** and click **"Poll now"**.

⌛ Wait 10–30 seconds, then poll again.

✅ If the app's backend visited your payload, you will see:

* DNS interaction
* HTTP interaction (optional)

📸 Screenshot placeholder: 
![ssrf](https://github.com/Kabilala/ssrf/blob/main/lab3/lab3.png)

---

### 4. 🏁 Lab Solved

Once Burp detects an interaction, the lab will be marked as **solved**.


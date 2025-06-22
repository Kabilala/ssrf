
# README — Basic SSRF Against the Local Server

## Overview

This lab demonstrates a **Server-Side Request Forgery (SSRF)** vulnerability where the application allows an attacker to make the server send HTTP requests to arbitrary URLs, including internal services like `localhost`.

## Objective

Exploit the SSRF vulnerability by manipulating a URL parameter to access an internal service running on the local server.

## Target Parameter

* `stockApi` — URL parameter in a POST request responsible for fetching stock details.

## Steps to Exploit

1. **Intercept the POST request** to `/product/stock` using Burp Suite or any HTTP proxy.

2. **Modify the `stockApi` parameter** in the request body to point to an internal URL, for example:

   ```
   http://localhost/admin
   ```

3. **Send the modified request**.

4. **Observe the response** confirming that the server accessed the internal service successfully.

## Impact

* Accessing internal services that should not be publicly accessible.
* Potential to escalate the attack to retrieve sensitive data, internal metadata, or perform further network reconnaissance.
* SSRF can be a gateway to more critical vulnerabilities like Remote Code Execution (RCE) or Cloud Metadata disclosure.

## Mitigation Recommendations

* Implement strict whitelist validation on URLs accepted by the server.
* Block requests to internal IP ranges (e.g., 127.0.0.1, 10.x.x.x, 192.168.x.x).
* Sanitize and validate user inputs rigorously on the server side.
* Use network segmentation and firewall rules to restrict server access.

## Conclusion

This basic SSRF lab highlights how unvalidated URL inputs can lead to significant security risks by enabling attackers to force the server to make arbitrary requests, including to sensitive internal resources.

---

If you want, I can help you generate a polished PDF or Markdown file ready for GitHub or LinkedIn! Also, ready to tackle the next SSRF challenge? 💥
Gotcha! Here’s a clean, professional README in English for your **Basic SSRF against the local server** lab — perfect for your portfolio, GitHub repo, or just to keep track of your pentesting journey.

---

# README — Basic SSRF Against the Local Server

## Overview

This lab demonstrates a **Server-Side Request Forgery (SSRF)** vulnerability where the application allows an attacker to make the server send HTTP requests to arbitrary URLs, including internal services like `localhost`.

## Objective

Exploit the SSRF vulnerability by manipulating a URL parameter to access an internal service running on the local server.

## Target Parameter

* `stockApi` — URL parameter in a POST request responsible for fetching stock details.

## Steps to Exploit

1. **Intercept the POST request** to `/product/stock` using Burp Suite or any HTTP proxy.

2. **Modify the `stockApi` parameter** in the request body to point to an internal URL, for example:

   ```
   http://localhost/admin
   ```

3. **Send the modified request**.

4. **Observe the response** confirming that the server accessed the internal service successfully.

## Impact

* Accessing internal services that should not be publicly accessible.
* Potential to escalate the attack to retrieve sensitive data, internal metadata, or perform further network reconnaissance.
* SSRF can be a gateway to more critical vulnerabilities like Remote Code Execution (RCE) or Cloud Metadata disclosure.

## Mitigation Recommendations

* Implement strict whitelist validation on URLs accepted by the server.
* Block requests to internal IP ranges (e.g., 127.0.0.1, 10.x.x.x, 192.168.x.x).
* Sanitize and validate user inputs rigorously on the server side.
* Use network segmentation and firewall rules to restrict server access.

## Conclusion

This basic SSRF lab highlights how unvalidated URL inputs can lead to significant security risks by enabling attackers to force the server to make arbitrary requests, including to sensitive internal resources.


🖼️ Screenshot
(Insert a screenshot showing the slow server response or Burp timing — if you captured it)

md
Copy
Edit
![ssrf](https://github.com/Kabilala/ssrf/blob/main/lab1/lab1.png)

# Phishing-Email-Analysis
## Contents

1. Executive Summary
2. Email Metadata Analysis 
    - Suspicious Elements
3. Key Findings 
    - A. Email Header Analysis
    - B. Embedded URL Analysis
    - C. IP Reputation Analysis
4. Threat Assessment
5. Recommendations
6. Conclusion

**1. Executive Summary**

An email appearing to impersonate OpenSea was received and forwarded to the phishing reporting mailbox. Upon inspection, the email originates from a third-party domain (`jfp.events`) and was routed via Amazon SES infrastructure. Although standard authentication checks (SPF, DKIM, DMARC) appear to have passed, the combination of indicators warrants further scrutiny due to the unusual sending domain and structure of the message.

---

## **2. Email Metadata Analysis**

| **Field** | **Value** |
| --- | --- |
| **From** | `OpenSea <noreply@jfp.events>` |
| **To** | `phishing@pot` |
| **Subject** | `An offer has just been submitted on an item on your listing [0xDa161b]` |
| **Date** | `Fri, 1 Mar 2024 21:49:55 +0000` |
| **Message-ID** | `<010e018dfbfec94a-...@ap-southeast-1.amazonses.com>` |

![image.png](attachment:317386a1-851e-445f-9c7b-d5ebc2c563df:image.png)

### **Suspicious Elements**

1. **Sender Domain:**
    - **`noreply@jfp.events`** is not an official OpenSea domain (legitimate OpenSea emails use **`opensea.io`**).
2. **Typos & Inconsistencies:**
    - "Offered Price**e**" (extra "e").
    - Poor grammar and formatting (e.g., "Offer Details" repeated).
3. **Fake Transaction Details:**
    - NFT item **`#738498`** and offer **`2.4875 ETH`** are likely fabricated.

## 3. Key Findings

### **A. Email Header Analysis**

| **Header Field** | **Details** |
| --- | --- |
| **Return-Path** | `amazonses.com` |
| **Sending IP** | `23.251.232.4` (Amazon SES - Singapore Region) |
| **SPF** | Pass (`ap-southeast-1.amazonses.com`) |
| **DKIM** | Pass (`jfp.events` and `amazonses.com`) |
| **DMARC** | Best guess pass |
| **Microsoft Spam Confidence Level (SCL)** | 5 (Likely Spam) |
| **Authentication Results** | SPF=pass, DKIM=pass, CompAuth=pass |
- **SPF, DKIM, DMARC:**
    - **SPF:** Pass (but sender domain **`jfp.events`** is not legitimate OpenSea).
    - **DKIM:** Pass (signed by **`jfp.events`** and **`amazonses.com`**).
    - **DMARC:** "Best Guess Pass" (weak enforcement).
- **X-SES-Outgoing:** Confirms Amazon SES was used (common in phishing).

![CleanShot 2025-05-06 at 15.56.35.png](attachment:f915d2d9-8cc7-4d44-92c2-600c5e16bca5:CleanShot_2025-05-06_at_15.56.35.png)

### **B. Embedded Url Analysis**

**Malicious Link:**

- The "Review Offer" button links to:
    
    **`http://www.sureingenieria.com.ar/sax/ZeroBot.php`**
    
- This domain was flagged as a **security risk** by Symantec.

| **Indicator** | **Analysis** |
| --- | --- |
| **Domain:** **`sureingenieria.com.ar`** | Registered in Argentina (not related to OpenSea). |
| **Hosting:** OVH SAS (Canada) | Unusual for a financial service. |
| **Symantec Rating:**  | "Business/Economy and **Compromised Sites**" (High risk). |
| **PhishTank** | No prior reports, but URL structure is suspicious (**`/sax/ZeroBot.php`**). |
| **URLScan.io** | No classification, but contacts multiple external IPs. |
| **VirusTotal** | No security vendors flagged the Url. |

**Whois**

![image.png](attachment:82c43e82-2ddd-44d3-8964-5ac5067a1573:image.png)

**Symantec**

img src= 'Folder/symantec.png'
**PhishTank**

![image.png](attachment:bc6a35be-1c0a-43e9-9e72-5f3e0b9485d0:image.png)

**URLScan.io**

![image.png](attachment:7075d549-f12f-441b-80b5-68e3c509a586:image.png)

**VirusTotal**

![CleanShot 2025-05-06 at 15.16.37.png](attachment:25e92e84-f12e-470e-9df9-f7cb41938463:CleanShot_2025-05-06_at_15.16.37.png)

### **C. IP Reputation Analysis**

**IP:** **`23.251.232.4`** (Amazon SES)

- **Legitimate Use:** AWS Simple Email Service (SES) is valid but frequently exploited by attackers.
- **Abuse History:** 12 reports (low confidence of abuse, but SES IPs are often rotated to evade detection).
- **Recommendation:** Block all emails from **`jfp.events`** and monitor AWS SES traffic for spoofing.

![CleanShot 2025-05-06 at 16.06.15.png](attachment:e601d927-ecc8-46eb-a5cb-105f6811bca1:CleanShot_2025-05-06_at_16.06.15.png)

## **4. Threat Assessment**

- **Risk Level:** **High** (Targets NFT traders with financial incentives).
- **Attack Type:** Credential Harvesting / Malware Delivery.
- **Objective:** Likely aims to steal OpenSea login credentials or cryptocurrency.

## **5. Recommendations**

1. **Do not click** any links or buttons in the email.
2. **Verify** NFT offers directly on OpenSea’s official website.
3. **Report** similar emails to IT security.
4. **Block the Domain:** Add **`sureingenieria.com.ar`** and **`jfp.events`** to email filters.
5. **Alert Users:** Send a company-wide warning about this phishing attempt.
6. **Enhance Email Security:** Enforce **strict DMARC policies** to prevent domain spoofing.
7. **Monitor Logs:** Check for any users who may have interacted with the email.

## **6. Conclusion**

This is a **high-confidence phishing attempt** impersonating OpenSea. The email leverages urgency (NFT offer) to trick victims into clicking a malicious link. Immediate action is required to prevent compromise.

**Prepared by:** Samuel Salami

**Date**: 06/05/2025

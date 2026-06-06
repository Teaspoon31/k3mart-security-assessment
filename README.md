# K3 Mart Security Assessment (Authorized)

## 📋 Project Overview
**Date:** June 2026  
**Role:** Security Tester (Authorized)  
**Type:** External Infrastructure & Web Application Security Assessment  
**Target:** `k3mart.store`  

This repository documents a professional, **authorized** security assessment performed on the K3 Mart e-commerce infrastructure. The objective was to evaluate the platform's external perimeter, web application controls, API configurations, and client-side assets to identify potential vulnerabilities, misconfigurations, or data leaks.

> ⚠️ **Authorization Disclaimer:** This assessment was conducted with explicit, written permission from the site owner. Do not perform active scanning or reconnaissance against production systems without prior authorized consent.

---

## 🛠️ Skills & Tools Demonstrated

### Technical Skill Matrix
* **Network Security:** Port enumeration, service fingerprinting, firewall/proxy identification.
* **Web Application Security:** HTTP response header evaluation, authentication architecture testing, route protection verification.
* **Cloud & API Security:** Backend-as-a-Service (BaaS) endpoint analysis, Supabase configuration assessment.
* **Application Security:** Production JavaScript bundle analysis, client-side secret detection, static code review.
* **Risk Management:** Vulnerability classification, impact analysis, actionable remediation reporting.

### Toolset Utilized
* **Nmap:** Network reconnaissance and active service enumeration.
* **cURL / PowerShell:** Manual HTTP request manipulation and custom response payload scripting.
* **Browser Developer Tools:** DOM inspection, network waterfall monitoring, and header analysis.
* **Supabase Client Core:** Analysis of client-to-backend communication rules.

---

## 📊 Executive Summary

The security posture of the K3 Mart e-commerce platform is **strong** regarding its external perimeter. The integration of Cloudflare effectively obfuscates the true origin server and mitigates basic network-level exploits. Web-facing components follow modern security baselines, displaying no critical or high-risk vulnerabilities.

### Risk Distribution
* **Critical:** 0
* **High:** 0
* **Medium:** 2
* **Low:** 1

### Key Security Strengths
* **Edge Protection:** Cloudflare WAF and reverse-proxying properly absorb baseline scanning noise.
* **Transport Encryption:** Strict HTTPS enforcement backed by a robust HSTS policy.
* **Defensive Headers:** Comprehensive Content Security Policy (CSP) and frame-ancestor rules drastically reduce XSS and Clickjacking surfaces.
* **Access Control:** Critical administrative routes are securely isolated behind robust authentication gates.

---

## 🔍 Vulnerability & Findings Registry

### Summary Table
| Finding ID | Title | Severity | Status |
| :--- | :--- | :--- | :--- |
| **SEC-M-01** | Exposure of Multiple Non-Standard Network Ports | 🟡 Medium | Documented |
| **SEC-M-02** | Exposure of Supabase Anon Key Without Verified RLS | 🟡 Medium | Documented |
| **SEC-L-01** | Verbose Service Version Disclosure on Non-Standard Ports | 🟢 Low | Documented |

---

### SEC-M-01: Exposure of Multiple Non-Standard Network Ports
* **Severity:** Medium
* **Description:** Active network scanning revealed 12+ open TCP ports (including `2052`, `2082`, `2086`, `2095`, `8080`, `8443`, and `8880`). While routed through Cloudflare's proxy architecture, exposing unnecessary ports broadens the attack surface and increases the risk of edge-routing misconfigurations.
* **Impact:** Potential exploitation of underlying services if edge proxy rules are bypassed or misconfigured.
* **Remediation:** Enforce strict ingress firewall rules at the origin server. Close or drop traffic on all network ports not explicitly required to serve the core web application (typically restricting to standard HTTP/HTTPS ports 80 and 443).

### SEC-M-02: Exposure of Supabase Anon Key Without Verified RLS
* **Severity:** Medium
* **Description:** Static analysis of the production client-side JavaScript bundle (`index-DAFLfPo2.js`) confirmed the inclusion of the Supabase `anon` API key. While exposing an anonymous public key is expected behavior in BaaS architectures, it demands strict database-level reinforcement.
* **Impact:** If Row Level Security (RLS) policies are not meticulously configured on every database table, malicious actors can utilize this public key to read, alter, or delete arbitrary backend records.
* **Remediation:** Perform a comprehensive database schema audit using the Supabase CLI. Ensure that **Row Level Security (RLS)** is enabled on every single table, and that data modification permissions are bound strictly to verified, authenticated user sessions.

---

## 📁 Repository Directory Structure
```text
k3mart-security-assessment/
├── README.md                 # Executive summary and findings registry
├── methodology.md            # Detailed technical testing phases
├── findings.md               # Deep-dive analysis and code evidence
├── report.pdf                # Consolidated, formal security report
└── screenshots/
    ├── nmap-scan.png         # Evidence of open port responses
    └── security-headers.png  # Evidence of active defensive headers

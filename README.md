# Networkwalks — Week 4 Penetration Testing Project

## Cybersecurity Internship — Week 4

This repository documents my Week 4 practical cybersecurity project as part of my internship at Networkwalks.

The project involved an authorized, educational black-box penetration-testing exercise against the assigned target, focusing on reconnaissance, web enumeration, authentication testing, PDF password cracking, metadata analysis, and identifying exposed backup data.

> **Scope and Authorization:** Testing was restricted to the assigned target, `https://medirozahospital.com`, under the project's written authorization. Activities outside the defined scope, social engineering, and denial-of-service testing were excluded.

---

## Objectives

- Perform reconnaissance and web enumeration.
- Identify exposed directories and authentication weaknesses.
- Test the assigned login functionality.
- Retrieve and crack the passwords of the three assigned PDF reports.
- Examine PDF metadata for further investigation clues.
- Identify the exposed database backup and assess the security impact.
- Document findings, risk levels, and remediation recommendations.

## Tools Used

- Kali Linux
- cURL
- WHOIS
- NSLookup
- WhatWeb
- Nmap/Zenmap (previous lab experience)
- Networkwalks Hash Calculator
- Networkwalks Password Cracker
- John the Ripper (JTR)
- qpdf
- ExifTool
- GitHub

---

## Methodology

The project followed these stages:

1. Reconnaissance
2. Web and directory enumeration
3. Authentication and SQL injection testing
4. Authorized patient-portal access testing
5. PDF hash extraction and password cracking
6. PDF decryption and metadata analysis
7. Backup directory and database exposure assessment
8. Risk assessment and remediation planning

---

# Module 1 — Initial Access and Web Testing

## 1. Reconnaissance

### Screenshot 01 — WHOIS
![WHOIS reconnaissance](Screenshots/01-whois.png)

### Screenshot 02 — DNS Lookup
![DNS lookup](Screenshots/02-nslookup.png)

### Screenshot 03 — WhatWeb
![WhatWeb results](Screenshots/03-whatweb.png)

### Screenshot 04 — HTTP Headers
![HTTP headers](Screenshots/04-curl-headers.png)

## 2. Web Enumeration

### Screenshot 05 — robots.txt
![robots.txt](Screenshots/05-robots.txt.png)

### Screenshot 06 — Sitemap
![Sitemap](Screenshots/06-sitemap.png)

### Screenshot 07 — Patient Directory
![Patient directory](Screenshots/07-patient-directory.png)

### Screenshot 08 — Patient Reports Directory
![Patient reports directory](Screenshots/08-patient-reports-403.png)

### Screenshot 09 — Download Endpoint
![Download endpoint](Screenshots/09-download-redirect.png)

### Screenshot 10 — Patient Login Page
![Patient login page](Screenshots/10-patient-login.png)

### Screenshot 11 — Staff Directory
![Staff directory](Screenshots/11-staff-directory.png)

### Screenshot 12 — Staff Login Page
![Staff login page](Screenshots/12-staff-login.png)

### Screenshot 13 — Error Log Access Test
![Error log access test](Screenshots/13-error-log-403.png)

### Screenshot 14 — Portal Redirect
![Portal redirect](Screenshots/14-portal-redirect.png)

## 3. Authentication Testing

### Screenshot 15 — Invalid Username Response
![Invalid username response](Screenshots/15-patient-login-invalid-user.png)

### Screenshot 16 — SQL Error Disclosure
![SQL error disclosure](Screenshots/16-sqli-error.png)

### Screenshot 17 — Initial Authentication Bypass Test
![Initial authentication bypass test](Screenshots/17-sqli-auth-bypass-failed.png)

### Screenshot 18 — Exposed Old Directory
![Old directory listing](Screenshots/18-old-directory-db-backup.png)

### Screenshot 19 — Database Backup Inspection
![Database backup inspection](Screenshots/19-db-backup-head.png)

### Screenshot 20 — SQL Injection Authentication Bypass
![SQL injection authentication bypass](Screenshots/20-sqli-login-bypass.png)

### Screenshot 21 — Patient Portal Access
![Patient portal](Screenshots/21-patient-portal.png)

### Screenshot 22 — Retrieved PDF Reports
![Retrieved PDF reports](Screenshots/22-three-patient-reports.png)

---

# Module 2 — PDF Password Cracking and Metadata Analysis

## 1. PDF Hash Extraction

### Screenshot 23 — PDF Files
![PDF files](Screenshots/23-pdf-files.png)

### Screenshot 24 — Report 1 Hash
![Report 1 hash](Screenshots/24-report1-hash.png)

### Screenshot 25 — Report 2 Hash
![Report 2 hash](Screenshots/25-report2-hash.png)

### Screenshot 26 — Report 3 Hash
![Report 3 hash](Screenshots/26-report3-hash.png)

## 2. Password Cracking

### Screenshot 27 — Report 1 Password Recovery
![Report 1 password recovery](Screenshots/27-report1-cracked.png)

### Screenshot 28 — Report 2 Password Recovery
![Report 2 password recovery](Screenshots/28-report2-cracked.png)

### Screenshot 29 — Initial Report 3 Wordlist Attempt
![Report 3 initial wordlist attempt](Screenshots/29-report3-wordlist-failed.png)

### Screenshot 30 — Report 3 JTR Wordlist Result
![Report 3 JTR result](Screenshots/30-report3-jtr-cracked.png)

## 3. PDF Decryption

### Screenshot 31 — Report 1 Decrypted
![Report 1 decrypted](Screenshots/31-report1-open.png)

### Screenshot 32 — Report 2 Decrypted
![Report 2 decrypted](Screenshots/32-report2-open.png)

### Screenshot 33 — Report 3 Decrypted
![Report 3 decrypted](Screenshots/33-report3-open.png)

## 4. Metadata Analysis

### Screenshot 34 — Report 3 Metadata
![Report 3 metadata](Screenshots/34-report3-metadata.png)

The metadata provided an investigation clue pointing toward the old backup directory. This clue was followed during the next stage of the assessment.

---

# Module 3 — Critical Data Exposure

## 1. Backup Directory Discovery

### Screenshot 35 — `/old/` Directory Listing
![Old directory listing](Screenshots/35-old-directory.png)

The directory listing exposed a database backup file, indicating that a backup had been left accessible through the web server.

## 2. Database Backup Retrieval

### Screenshot 36 — Downloaded Database Backup
![Database backup](Screenshots/36-db-backup.png)

### Screenshot 37 — Database Tables
![Database tables](Screenshots/37-db-tables.png)

The backup contained staff and shareholder tables. This represented a serious confidentiality risk because internal records were stored in a publicly accessible location.

## 3. Sensitive Data Exposure Assessment

### Screenshot 38 — Staff Salary Records
![Staff salary evidence](Screenshots/38-staff-salaries.png)

### Screenshot 39 — Shareholder Records
![Shareholder evidence](Screenshots/39-shareholders.png)

The assessment identified exposure of staff salary information and shareholder details. Sensitive values are intentionally not reproduced in this public report.

## 4. Metadata Correlation

### Screenshot 40 — Staff Record and Metadata Correlation
![Metadata correlation](Screenshots/40-jameel-metadata-connection.png)

The author identifier found in the PDF metadata was correlated with a matching staff record in the exposed backup. This provided context linking the metadata clue to the backup exposure.

---

# Findings and Risk Ratings

| ID | Finding | Risk |
|---|---|---|
| F-01 | Username enumeration | Medium |
| F-02 | SQL injection authentication bypass | Critical |
| F-03 | Patient PDF reports accessible following authentication bypass | High |
| F-04 | Weak PDF passwords | High |
| F-05 | Sensitive PDF metadata disclosure | Medium |
| F-06 | Publicly accessible backup directory | Critical |
| F-07 | Staff salary and shareholder data exposure | Critical |

*Risk ratings are based on the supplied project solution and the demonstrated lab scenario.*

## Recommendations and Remediation

1. **Prevent username enumeration:** Use consistent authentication error messages regardless of whether a username exists.
2. **Fix SQL injection:** Use parameterized queries or prepared statements, validate input, and avoid dynamically concatenating user input into SQL queries.
3. **Strengthen access controls:** Store confidential reports outside the public web root and enforce server-side authorization for every download.
4. **Improve PDF protection:** Use strong, unique passwords where encryption is required, and manage credentials securely.
5. **Remove sensitive metadata:** Sanitize document metadata before publishing files.
6. **Secure web-server configuration:** Disable directory listing where it is not required.
7. **Protect database backups:** Remove backup files from public web directories and store them in access-controlled locations.
8. **Review exposure and access:** Investigate potentially accessed records, restrict access, and follow the organization's incident-response and data-protection procedures.

---

# Executive Summary

The Week 4 assessment demonstrated how reconnaissance, authentication weaknesses, insufficient document protection, and publicly accessible backup files can combine to expose confidential information.

The main risks identified in the supplied lab scenario were SQL injection leading to authentication bypass, weak PDF passwords, sensitive metadata, and an exposed database backup containing staff and shareholder records.

The assessment highlights the importance of secure coding, proper access control, secure backup handling, document sanitization, and regular security testing.

## Conclusion

This project provided practical experience with black-box testing, web enumeration, authentication testing, PDF password recovery, metadata analysis, evidence collection, risk classification, and remediation planning.

All work was performed within the assigned educational project scope.

---

## Evidence

Screenshots are stored in the [`Screenshots`](Screenshots/) directory. Each screenshot is referenced in the relevant section above.

**Repository:** Networkwalks Week 4 Penetration Testing Project

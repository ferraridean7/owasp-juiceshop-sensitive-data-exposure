# Security Requirements Document (SRD) & Security Response Plan (SRP)
## Case Study: Sensitive Data Exposure & Directory Listing Remediation

> **DISCLAIMER & ETHICAL STATEMENT**  
> This project is a security analysis study, risk governance framework mapping, and security requirements engineering exercise (GRC Documentation) using **OWASP Juice Shop** (a locally hosted lab environment intentionally designed for educational purposes). This document does not represent security testing conducted on unauthorized or commercial enterprise systems.

---

## 1. Executive Summary

This document outlines the technical security evaluation, risk assessment, **Security Requirements Document (SRD)**, and **Security Response Plan (SRP)** addressing an identified **Sensitive Data Exposure** and **Directory Listing** vulnerability within the OWASP Juice Shop public file storage route (`/ftp/`).

The primary objective of this documentation is to deliver structured engineering requirements for robust Access Control Lists (ACLs), enforce server-level directory indexing restrictions, and establish actionable incident response procedures to prevent unauthorized leakage of confidential internal corporate documents (e.g., strategic acquisition roadmaps).

---

## 2. Risk & Impact Analysis & Risk Treatment Plan

| Parameter | Initial Assessment (Pre-Mitigation) | Post-Mitigation (Residual Risk) |
| :--- | :--- | :--- |
| **Vulnerability** | Exposure of Sensitive Information (CWE-200) / Improper Access Control (CWE-284) | Remediated / Neutralized |
| **CVSS v3.1 Score** | **5.3 (MEDIUM)** (`CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N`) | **0.0 (NONE)** |
| **Risk Treatment Strategy** | N/A | **Risk Reduction (Mitigation)** |
| **Risk Owner** | Lead Backend Engineer / System Administrator | Head of Information Security (Sign-off) |
| **Remediation SLA** | **7 Business Days** (Medium Severity Standard) | Verified & Closed |

### 2.1 Residual Risk Justification
By enforcing strict authentication boundaries, disabling directory indexing (directory listing) at the web server level, and restricting public route access to `/ftp/`, unauthorized file enumeration is completely neutralized. Confidential documents are protected behind proper access control lists (ACLs), reducing residual risk to zero.

---

## 3. Compliance Framework Mapping & ISMS Alignment

To ensure a comprehensive enterprise risk governance approach, this technical finding is mapped across both **ISO/IEC 27001:2022 (ISMS Clauses & Annex A Controls)** and **NIST CSF v2.0 Functions**.

---

### 3.1 Framework Control Mapping Table

| Standard Framework | Control ID & Name | Strategic Alignment Description |
| :--- | :--- | :--- |
| **ISO/IEC 27001:2022** | **Clause 4.1 & 4.2** (Context & Interested Parties) | Identifies internal/external issues regarding public data leakage and aligns with stakeholder expectations for data confidentiality. |
| **ISO/IEC 27001:2022** | **Clause 6.1.2 & 6.1.3** (Risk Assessment & Treatment) | Establishes formal risk acceptance criteria (e.g., CVSS 5.3 Medium threshold) and determines risk treatment options via SoA justification. |
| **ISO/IEC 27001:2022** | **Annex A.5.15** (Access Control) | Mandates logical access rules to block unauthenticated public entry to internal application storage routes. |
| **ISO/IEC 27001:2022** | **Annex A.8.9** (Configuration Management) | Requires secure server hardening to remediate dangerous default misconfigurations (e.g., disabling web server directory listing). |
| **ISO/IEC 27001:2022** | **Annex A.8.26** (Application Security Requirements) | Specifies that application features handling sensitive assets must enforce strict authorization checks prior to data rendering. |
| **NIST CSF v2.0** | **PR.AA-01** (Authentication & Access Control) | Enforces identity verification and access restriction policies across public endpoint assets. |
| **NIST CSF v2.0** | **PR.DS-01** (Data Security) | Safeguards sensitive corporate data against unauthorized disclosure and unauthenticated enumeration. |

---

### 3.2 End-to-End ISMS Execution Lifecycle (ISO/IEC 27001:2022 Alignment)

Engineering teams and risk managers must execute the following structured lifecycle to remediate CWE-200 within the Information Security Management System (ISMS):

#### Phase 1: Context & Scope Boundaries (Clause 4)
* **Internal/External Expectations (Clause 4.1 & 4.2)**: Document internal business requirements and regulatory obligations regarding the non-disclosure of strategic corporate documentation (e.g., acquisition plans).
* **ISMS Scope Definition (Clause 4.3)**: Explicitly define the ISMS boundary to include web applications, public routing layers, and backend document file servers (`/ftp/`).

#### Phase 2: Risk Assessment & Treatment Planning (Clause 6)
* **Asset Identification & Risk Criteria (Clause 6.1.2)**: Register sensitive file assets (`acquisitions.md`) within the Corporate Asset Inventory. Evaluate the loss of confidentiality resulting from CWE-200 using the CVSS v3.1 framework (**5.3 MEDIUM**).
* **Statement of Applicability (SoA) Justification (Clause 6.1.3.d)**: Formally document the inclusion of Annex A controls (A.5.9 Asset Inventory, A.5.12/A.5.13 Classification & Labelling, A.5.15 Access Control, A.8.3 Information Access Restriction, A.8.9 Configuration Management, and A.8.26 Application Security Requirements) within the organization's SoA.

#### Phase 3: Operational Execution & Technical Remediation (Clause 8)
* **Risk Treatment Implementation (Clause 8.1 & 8.3)**: Execute technical countermeasures by disabling directory listing at the web server engine level, moving sensitive assets out of publicly accessible web roots, and enforcing mandatory authentication controls.
* **Technical Vulnerability Management (Clause 8.8)**: Conduct routine automated vulnerability scans and static analysis checks to detect unauthenticated route exposures across staging and production environments.

#### Phase 4: Performance Evaluation & Continuous Improvement (Clause 9 & 10)
* **Internal Audit & Monitoring (Clause 9.2)**: Perform periodic internal ISMS audits to verify that directory indexing controls and route access control lists remain active and effective.
* **Nonconformity & Corrective Action (Clause 10.2)**: When sensitive data exposures are identified, trigger a formal Nonconformity Report (NCR) to perform root cause analysis and implement corrective actions to prevent recurrence across other application routes.

---

## 4. Proof of Concept (PoC) & Execution Evidence

1. **Vulnerability Mechanics**: 
   The web server suffers from a misconfiguration that enables directory indexing (Directory Listing) on the public storage endpoint (`/ftp/`). This allows unauthenticated external users to browse internal server file structures, enumerate sensitive corporate backups, and access confidential files directly without access control enforcement (CWE-200 / CWE-284).

2. **Exploitation & Enumeration Workflow**:
   * **Step 1 (Directory Enumeration)**: Navigating to the public storage endpoint exposes the complete file directory tree listing structure, including sensitive files such as `acquisitions.md`, `coupons_2013.md.bak`, and database/keepass backups (`incident-support.kdbx`).
   * **Step 2 (Direct File Access)**: Appending the target file name to the URI path permits direct retrieval and rendering of confidential corporate data without requiring user session authentication or authorization tokens.

3. **Execution Evidence (Embedded Visuals)**:

   * **Discovery Phase (Unprotected Directory UI Discovery)**:  
     Discovery of the unprotected file storage route via UI link inspection.  
     ![Discovery of Unprotected Directory via UI Link](./data_exposure/Figure%201%20-%20Discovery%20of%20Unprotected%20Directory%20via%20UI%20Link.png)
     Figure 1 - Discovery of Unprotected Directory via UI Link

   * **Exploitation Phase (Directory Listing / Server Misconfiguration)**:  
     Server misconfiguration enabling unauthenticated directory listing and file enumeration at `/ftp/`.  
     ![Server Misconfiguration Enabling Directory Listing](./data_exposure/Figure%202%20-%20Server%20Misconfiguration%20Enabling%20Directory%20Listing.png)

   * **Impact Verification (Sensitive Data Exposure)**:  
     Direct retrieval of the confidential internal document `acquisitions.md` via http://localhost:3000/ftp/acquisitions.md.  
     ![Sensitive Data Exposure via acquisitions.md](./data_exposure/Figure%203%20-%20Sensitive%20Data%20Exposure%20via%20acquisitions.md.png)

---

## 5. Security Requirements Document (SRD) & Technical Remediation

To permanently neutralize **Sensitive Data Exposure (CWE-200)** and **Improper Access Control (CWE-284)** vulnerabilities originating from the public file storage route (`/ftp/`), the engineering and server administration teams must implement the following mandatory technical requirements and secure configuration controls.

---

### 5.1 Server-Level Hardening & Directory Indexing Controls

Web server engines (such as NGINX, Apache, or Express static middleware) must be strictly hardened to prevent automatic directory enumeration.

1. **Disable Directory Listing (Indexing)**:
   * **Apache Implementation**: Ensure `Options -Indexes` is explicitly declared within the virtual host configuration block for public document directories.
   * **Nginx Implementation**: Ensure `autoindex off;` is set within the server or location context to block automatic file tree rendering.
   * **Express.js / Node.js Implementation**: Avoid exposing raw static middleware (`express.static`) directly to sensitive root directories without custom route validation or access control middleware.

2. **Secure Configuration Example (Nginx Hardening)**:
   ```nginx
   location /ftp/ {
       # Disable automatic directory listing/indexing
       autoindex off;
       
       # Enforce authentication requirement before serving assets
       auth_request /auth-verify;
   }

### 5.2 Access Control Lists (ACLs) & Authorization Framework

Confidential internal documents (such as 'acquisitions.md' and database backups) must never reside within unauthenticated public web roots.
  * **Role-Based Access Control (RBAC)** : Enforce strict authorization checks ensuring that only authenticated users possessing authorized administrative roles can access directory routes containing sensitive documentation.
  * **Asset Segregation**: Move sensitive corporate documents, roadmaps, and configuration backups out of public directories (e.g., `/ftp/`) into secure internal storage locations or encrypted object storage solutions with strict IAM (Identity and Access Management) policies.
    
### 5.3 Automated Security Controls (CI/CD Pipeline Integration)
To prevent regression and ensure proactive security alignment with ISO/IEC 27001:2022 Annex A.8.9 (Configuration Management) and NIST CSF v2.0 PR.DS-01 (Data Security), automated validation checks must be integrated into the deployment pipeline.
  * **Infrastructure as Code (IaC) Scanning**:
Utilize automated static analysis tools (e.g., Checkov, Trivy, or tfsec) within GitHub Actions workflows to scan server configuration files (Dockerfiles, Nginx configs, Express routing scripts) for enabled directory indexing flags (`autoindex` on or missing access controls).
  * **Build-Time Verification**:
Pipeline configurations must automatically fail if a pull request introduces unauthenticated static routes pointing to sensitive file extensions (`.md, .bak, .kdbx, .yml`).

---

## 6. Security Response Plan (SRP) - Operational Incident Handling Procedures

When a security incident involving unauthorized access to confidential internal documents (e.g., unauthorized scraping or public exposure of files via `/ftp/`) is detected by Security Operations Center (SOC) monitoring systems or external whistleblowers, the following structured incident response workflow must be executed by the Incident Response Team (IRT).

---

### Phase 1: Triage & Identification

1. **Log Analysis & Pattern Matching**: 
   Query web server access logs (e.g., NGINX, Apache, or Express access logs) for suspicious request patterns targeting public storage endpoints:
   * **Indicators of Compromise (IoC)**: High-frequency HTTP `GET` requests targeting sensitive file assets (`/ftp/acquisitions.md`, `/ftp/incident-support.kdbx`, `/ftp/package.json.bak`) originating from unauthenticated external IP addresses.
2. **Blast Radius & Impact Assessment**:
   * Determine the exact volume and names of confidential files downloaded during the exposure window.
   * Assess the business impact associated with the leaked data (e.g., premature disclosure of corporate acquisition strategies or leaked database credentials).

---

### Phase 2: Containment (Immediate Mitigation)

1. **Emergency Server Hardening / Configuration Rollback**:
   * Immediately apply server-level patches to disable directory indexing (`autoindex off;` / `Options -Indexes`) on the web server.
   * Temporarily quarantine or unpublish sensitive files from the public storage route pending formal review.
2. **Edge Access Blocking (WAF / Firewall Rules)**:
   * Deploy temporary Web Application Firewall (WAF) rate-limiting or URI blocking rules to restrict direct public enumeration attempts against the `/ftp/` path.

---

### Phase 3: Eradication & Recovery

1. **Secure Storage Relocation**:
   * Relocate all internal corporate documents, backups, and markdown files completely out of the public web root into secure internal object storage protected by mandatory Access Control Lists (ACLs).
2. **Data Integrity & Credential Rotation**:
   * If sensitive configuration or credential backup files (e.g., `.bak`, `.kdbx`) were accessed during the incident, immediately revoke and rotate all associated database credentials, API keys, and access secrets.
3. **Recovery Validation**:
   * Conduct automated endpoint validation tests and manual browsing checks to verify that directory listing returns a `403 Forbidden` or `404 Not Found` status code, and that direct unauthenticated retrieval of sensitive assets is fully blocked.

---

### Phase 4: Post-Incident Activity (Lessons Learned & Compliance Reporting)

1. **Root Cause Analysis (RCA)**:
   Conduct a formal post-mortem review within 5 business days to determine how misconfigured directory permissions bypassed development validation checks.
2. **Enterprise Risk Register Update**:
   Record the incident timeline, root cause, CVSS severity (**5.3 MEDIUM**), and business impact in the Enterprise Risk Register, mapped directly to **ISO/IEC 27001:2022 Annex A.8.9 (Configuration Management)**.
3. **Governance & Exception Policy**:
   If the engineering team cannot immediately refactor legacy file storage routes due to operational blockers, a formal **Risk Acceptance / Exception Request** must be submitted to the Risk Committee, accompanied by compensating controls (such as strict edge-level IP whitelisting) subject to daily audit review by the Head of Information Security.

---

## About the Author

Ex-Product Manager pivoting into Governance, Risk, and Compliance (GRC). This repository demonstrates the practical application of technical vulnerability research (CWE-200 / CWE-284) mapped directly to enterprise security controls and compliance frameworks (ISO/IEC 27001 & NIST CSF v2.0).

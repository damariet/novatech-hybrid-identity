# Hybrid Identity Lab — Active Directory + Microsoft Entra ID
### On-premises AD integrated with cloud identity using Entra Connect Sync

**NovaTech Solutions — Identity & Access Management Lab**  
Built by Damarie Tholley | April 2026

---

## What This Project Does

This project demonstrates a complete hybrid identity architecture that mirrors how enterprise organizations manage employee identities across both on-premises infrastructure and cloud resources. On-premises Active Directory is connected to Microsoft Entra ID (formerly Azure AD) using Entra Connect Sync, creating a unified identity that works seamlessly across both environments.

---

## Architecture Overview

```
On-Premises (VMware Workstation)          Cloud (Microsoft Azure)
─────────────────────────────────         ──────────────────────────────
Windows Server 2022                       Microsoft Entra ID (Free)
  └── Active Directory DS                   └── 9 synced users
        ├── homelab.local domain               ├── damarietholleyoutlook
        ├── IT_Staff (OU)                      │   .onmicrosoft.com
        │   ├── Alex Smith                     └── Conditional Access
        │   └── IT Admin                             ├── Policy 1 - MFA
        └── Regular_Users (OU)                       ├── Policy 2 - Legacy Auth
            ├── Bob Jones                            └── Policy 3 - Locations
            ├── Mike Chen
            └── Sara Lee
                    │
                    │  Entra Connect Sync
                    │  (Password Hash Sync)
                    ▼
              Entra ID tenant
              (all users synced)
```

---

## What Was Built

### Phase 1 — On-Premises Active Directory

| Component | Configuration |
|---|---|
| OS | Windows Server 2022 Evaluation |
| Role | Active Directory Domain Services + DNS |
| Domain | homelab.local |
| Forest/Domain level | Windows Server 2016 |
| Organizational Units | IT_Staff, Regular_Users |
| Users created | Alex Smith, IT Admin, Bob Jones, Mike Chen, Sara Lee |

**Why OUs matter:** Organizational Units mirror how real companies structure directories — separating privileged IT accounts from standard users allows different Group Policy application, different sync scopes, and different Conditional Access targets.

---

### Phase 2 — Microsoft Entra ID + Entra Connect Sync

| Setting | Value |
|---|---|
| Sync method | Express settings (Password Hash Sync) |
| Sync agent | Microsoft Entra Connect (latest) |
| Source directory | homelab.local |
| Target tenant | damarietholleyoutlook.onmicrosoft.com |
| Objects synced | 9 users synced successfully |
| Password sync | Enabled (hash synchronization) |
| Auto upgrade | Enabled |

**What Password Hash Sync means:** Users sign in to cloud services using the same credentials they use on-premises. No separate cloud password to manage. This is the most common sync method for enterprise hybrid identity deployments.

**Verification:** After sync, all users appear in Entra ID with `On-premises synced: Yes` confirming the hybrid connection is working.

---

### Phase 3 — Conditional Access Policies

All policies deployed in **report-only mode** — industry best practice before enforcement. Report-only evaluates every sign-in and logs what the policy would have done without actually blocking or requiring anything.

| Policy | Scope | Control | Zero Trust Pillar |
|---|---|---|---|
| Policy 1 - Require MFA for All Users | All users | Require MFA | Verify explicitly |
| Policy 2 - Block Legacy Authentication | All users | Block access | Assume breach |
| Policy 3 - Block High Risk Locations | All users | Block access | Assume breach |

**Why block legacy authentication?** Protocols like IMAP, POP3, and SMTP Auth were designed before MFA existed — they cannot complete a multi-factor challenge. This makes any account using them vulnerable to password spray attacks. Microsoft data shows the vast majority of password spray attacks use legacy auth specifically to bypass MFA.

**Why report-only mode?** Before enforcing any policy you need to see its real-world impact against live traffic. Report-only lets you verify behavior for 1-2 weeks then switch to enforced once you are confident there are no false positives.

---

## Key Concepts Demonstrated

**Hybrid identity** — a single identity that works across both on-premises AD and cloud services. Users authenticate once and access both environments with the same credentials.

**Password Hash Synchronization** — AD password hashes are synced to Entra ID so users can sign in to cloud apps with their on-premises credentials without a separate cloud password.

**Conditional Access** — policy engine that makes access decisions based on who the user is, what they are accessing, where they are signing in from, and what device they are using. The "if/then" of modern identity security.

**Report-only mode** — deploy policies safely by evaluating them against real traffic without enforcement. The standard approach for any production Conditional Access deployment.

---

## Prerequisites

- VMware Workstation or VirtualBox (free)
- Windows Server 2022 ISO (evaluation — free from Microsoft)
- Microsoft Azure free account (`azure.microsoft.com/free`)
- Entra ID free tenant (created automatically with Azure account)
- Microsoft Entra Connect download (from `entra.microsoft.com`)

---

## Verified Results

```
# PowerShell verification — all users confirmed in AD
Get-ADUser -Filter * -SearchBase "DC=homelab,DC=local" | Select Name, DistinguishedName

Name           DistinguishedName
----           -----------------
Alex Smith     CN=Alex Smith,OU=IT_Staff,DC=homelab,DC=local
IT Admin       CN=IT Admin,OU=IT_Staff,DC=homelab,DC=local
Bob Jones      CN=Bob Jones,OU=Regular_Users,DC=homelab,DC=local
Mike Chen      CN=Mike Chen,OU=Regular_Users,DC=homelab,DC=local
Sara Lee       CN=Sara Lee,OU=Regular_Users,DC=homelab,DC=local
```

All 9 users confirmed in Entra ID with `On-premises synced: Yes`

---

## Interview Talking Points

**"What is hybrid identity?"**
A hybrid identity bridges on-premises Active Directory with cloud identity providers like Microsoft Entra ID. Users get one set of credentials that works across both environments. Entra Connect Sync is the agent that keeps the two directories in sync automatically.

**"Why use Password Hash Sync instead of Federation?"**
Password Hash Sync is simpler, more resilient, and has no single point of failure. Federation requires an ADFS infrastructure that can go down and take cloud authentication with it. For most organizations PHS provides the right balance of security and simplicity.

**"What is Conditional Access?"**
Conditional Access is the policy engine that decides whether to allow, block, or challenge a sign-in based on conditions like user identity, group membership, location, device compliance, and app being accessed. It is how you enforce Zero Trust at the identity layer.

---

## Part of a Larger Lab Series

This project is the foundation for a series of enterprise IT lab projects:

1. **Hybrid Identity** ← you are here
2. **Zero Trust Access Gateway** — Conditional Access policies, Named Locations, RBAC groups
3. **Automated User Lifecycle** — PowerShell onboarding, offboarding, compliance reporting

---

*Built as part of a hands-on enterprise IT homelab. All company names and user data are fictional.*

# Microsoft Entra ID Identity Security Lab

## Overview
This portfolio project demonstrates the design, configuration, testing, and documentation of identity-security controls in a simulated Microsoft Entra ID environment for **Cortez Technology Solutions (CTS)**.

The lab focuses on Microsoft Entra Conditional Access. Eight policies were configured and validated with the **Conditional Access What If** tool using positive and negative test scenarios. Policies remained in **Report-only** mode during validation to support a staged, low-risk deployment approach.

> **Note:** CTS, its users, groups, applications, and project data are fictional and were created for this portfolio lab.

## Project Objectives
- Build a controlled Microsoft Entra ID identity-security test environment.
- Configure test users, security groups, and an MFA pilot population.
- Target Microsoft Authenticator for pilot authentication-method testing.
- Design Conditional Access controls for MFA, legacy authentication, location, privileged roles, device compliance, identity risk, and session management.
- Validate expected policy behavior with positive and negative What If scenarios.
- Document security findings, recommendations, limitations, and production rollout considerations.

## Technologies & Concepts
**Microsoft Entra ID** · **Conditional Access** · **Microsoft Authenticator** · **MFA** · **Identity Protection risk signals** · **Conditional Access What If** · **Report-only deployment** · **Privileged roles** · **Device compliance** · **Named locations** · **Session controls** · **Least privilege** · **Defense in depth** · **Zero Trust principles**

## Conditional Access Architecture

| Policy | Security Control | Primary Focus | Expected Control |
|---|---|---|---|
| **CA-001** | Require MFA Pilot | Pilot users/groups | Require MFA |
| **CA-002** | Block Legacy Authentication | Legacy/other client apps | Block access |
| **CA-003** | Block Untrusted Locations | Location | Block access |
| **CA-004** | Require MFA for Admins | Selected directory roles | Require MFA |
| **CA-005** | Require Compliant Device | Device/access scope | Require compliant device |
| **CA-006** | Require MFA for Sign-in Risk | Sign-in risk | Require MFA |
| **CA-007** | Require Password Change for User Risk | User risk | MFA/authentication strength + password change |
| **CA-008** | Sign-in Frequency | Session management | 12-hour sign-in frequency |

## Testing Methodology
The Conditional Access **What If** tool was used to evaluate policy behavior without production enforcement. Each policy was tested with an expected-apply scenario and an expected-not-apply scenario. This validated both the intended security control and the policy's assignments, exclusions, or conditions.

## Validation Results

| Policy | Positive Validation | Negative Validation | Result |
|---|---|---|---|
| **CA-001** | MFA applied to the pilot-user scenario | Non-pilot scenario did not receive CA-001 | Validated |
| **CA-002** | Legacy/other client scenario triggered Block access | Modern Browser scenario did not meet the client-app condition | Validated |
| **CA-003** | Untrusted-location scenario triggered Block access | Trusted location was excluded by location logic | Validated |
| **CA-004** | Targeted admin-role scenario triggered MFA | Nonmatching role/user scenario did not receive CA-004 | Validated |
| **CA-005** | Included scenario required a compliant device | Excluded-user scenario did not receive CA-005 | Validated |
| **CA-006** | Medium sign-in risk triggered MFA | No-risk scenario did not meet the risk condition | Validated |
| **CA-007** | High user risk triggered remediation controls | No-risk scenario did not meet the user-risk condition | Validated |
| **CA-008** | Targeted scenario received a 12-hour sign-in frequency | Excluded-user scenario did not receive CA-008 | Validated |

## Evidence

### Environment Setup
See [`Screenshots/01-Environment-Setup/`](Screenshots/01-Environment-Setup/) for evidence of test-user creation, security groups, MFA pilot membership, authentication-method baseline, Microsoft Authenticator targeting, and final pilot configuration.

### Conditional Access Policies
Evidence is organized by policy under [`Screenshots/02-Conditional-Access-Policies/`](Screenshots/02-Conditional-Access-Policies/):

- [`CA-001-Require-MFA-Pilot`](Screenshots/02-Conditional-Access-Policies/CA-001-Require-MFA-Pilot/)
- [`CA-002-Block-Legacy-Authentication`](Screenshots/02-Conditional-Access-Policies/CA-002-Block-Legacy-Authentication/)
- [`CA-003-Block-Untrusted-Locations`](Screenshots/02-Conditional-Access-Policies/CA-003-Block-Untrusted-Locations/)
- [`CA-004-Require-MFA-Admins`](Screenshots/02-Conditional-Access-Policies/CA-004-Require-MFA-Admins/)
- [`CA-005-Require-Compliant-Device`](Screenshots/02-Conditional-Access-Policies/CA-005-Require-Compliant-Device/)
- [`CA-006-Require-MFA-SignIn-Risk`](Screenshots/02-Conditional-Access-Policies/CA-006-Require-MFA-SignIn-Risk/)
- [`CA-007-Require-Password-Change-User-Risk`](Screenshots/02-Conditional-Access-Policies/CA-007-Require-Password-Change-User-Risk/)
- [`CA-008-Sign-In-Frequency`](Screenshots/02-Conditional-Access-Policies/CA-008-Sign-In-Frequency/)

## Security Findings
Key production considerations identified during the project include reviewing Report-only/sign-in results before enforcement, keeping exclusions narrowly scoped, protecting privileged identities, validating MFA and remediation readiness, confirming device compliance dependencies, reviewing legacy-authentication usage, maintaining emergency-access procedures, and treating trusted locations as one contextual signal rather than the sole basis for trust.

For the detailed analysis, see **[Security Findings & Recommendations](SECURITY-FINDINGS.md)**.

## Security Design Principles Demonstrated
- **Least privilege:** Temporary administrative access used for validation was removed after testing.
- **Defense in depth:** Identity, authentication, client-app, location, device, risk, and session controls are combined.
- **Staged deployment:** Pilot scoping, Report-only mode, and What If testing reduce deployment risk.
- **Zero Trust alignment:** Access decisions consider multiple identity and access signals rather than assuming network-based trust.
- **Evidence-based validation:** Positive and negative scenarios were documented before enforcement.

## Documentation
- [CTS Identity Security Project Documentation](Documentation/CTS-Identity-Security-Project-Documentation.docx)
- [Security Findings & Recommendations Report](Documentation/CTS-Identity-Security-Security-Findings-and-Recommendations.docx)
- [GitHub Security Findings](SECURITY-FINDINGS.md)

## Skills Demonstrated
- Microsoft Entra ID identity and group administration
- MFA and authentication-method pilot configuration
- Conditional Access policy design and scoping
- Positive and negative What If testing
- Privileged-role protection and least-privilege practices
- Legacy-authentication mitigation
- Named/trusted location controls
- Device-compliance access requirements
- Sign-in risk and user-risk policy logic
- Session-control configuration
- Security evidence collection and technical documentation

## Repository Structure
```text
Microsoft-Entra-Identity-Security-Lab/
├── README.md
├── SECURITY-FINDINGS.md
├── Documentation/
│   ├── CTS-Identity-Security-Project-Documentation.docx
│   └── CTS-Identity-Security-Security-Findings-and-Recommendations.docx
└── Screenshots/
    ├── 01-Environment-Setup/
    └── 02-Conditional-Access-Policies/
        ├── CA-001-Require-MFA-Pilot/
        ├── CA-002-Block-Legacy-Authentication/
        ├── CA-003-Block-Untrusted-Locations/
        ├── CA-004-Require-MFA-Admins/
        ├── CA-005-Require-Compliant-Device/
        ├── CA-006-Require-MFA-SignIn-Risk/
        ├── CA-007-Require-Password-Change-User-Risk/
        └── CA-008-Sign-In-Frequency/
```

## Project Limitations
This project is a **simulated portfolio lab** and does not represent a production deployment. What If testing validates Conditional Access policy evaluation logic but does not replace end-to-end production testing, user acceptance testing, operational monitoring, change management, or incident-response procedures. Risk- and device-based controls also depend on appropriate licensing, telemetry, device-management/compliance data, authentication readiness, and supporting remediation capabilities.

---
**Project focus:** Identity & Access Management · Microsoft Entra ID · Identity Security · Conditional Access

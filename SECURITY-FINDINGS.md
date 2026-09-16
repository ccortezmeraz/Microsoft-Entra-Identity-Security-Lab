# CTS Identity Security Project — Security Findings

## Overview

This project validates eight Microsoft Entra ID Conditional Access controls using **What If** testing. Policies were kept in **Report-only** mode during validation so targeting, exclusions, grant controls, and session controls could be evaluated before enforcement.

## Validation Summary

| Policy | Control | Evidence | State |
|---|---|---|---|
| CA-001 | MFA pilot | Positive/negative What If validation | Report-only |
| CA-002 | Legacy authentication block | Positive/negative What If validation | Report-only |
| CA-003 | Untrusted-location block | Positive/negative What If validation | Report-only |
| CA-004 | MFA for admins | Positive/negative What If validation | Report-only |
| CA-005 | Compliant device | Positive/negative What If validation | Report-only |
| CA-006 | Sign-in risk MFA | Positive/negative What If validation | Report-only |
| CA-007 | User-risk remediation | Positive/negative What If validation | Report-only |
| CA-008 | 12-hour sign-in frequency | Positive/negative What If validation | Report-only |

## Findings and Recommendations

### CA-001 — Require MFA Pilot

**Objective:** Validate a controlled MFA rollout to a designated pilot population before broader enforcement.

**Test evidence:** Positive test: Alex Johnson, in the MFA pilot population, was evaluated and the MFA policy applied. Negative test: Daniel Thompson, outside the targeted pilot population, was evaluated and the policy did not apply.

**Finding:** What If testing demonstrated intended user/group targeting and an MFA grant requirement for the pilot population.

**Recommendation:** Keep the policy in Report-only while validating pilot membership, authentication-method readiness, and sign-in impact. Expand enforcement in stages after reviewing results and maintaining an emergency-access exclusion.

### CA-002 — Block Legacy Authentication

**Objective:** Reduce exposure to authentication methods that do not support modern authentication controls such as MFA.

**Test evidence:** Positive testing used a legacy-authentication client scenario and showed the block policy applying. Negative testing used a modern browser scenario and showed the policy not applying because the client-app condition was not met.

**Finding:** The policy differentiated the targeted legacy client-app condition from a modern browser sign-in as designed.

**Recommendation:** Review sign-in logs for remaining legacy-authentication dependencies before enforcement. Remediate or replace required legacy clients, then move from Report-only to enforcement through a controlled change process.

### CA-003 — Block Untrusted Locations

**Objective:** Restrict access from locations outside the organization's defined trusted-location boundary.

**Test evidence:** Testing covered an untrusted-location scenario where the block control applied and a trusted/excluded-location scenario where the policy did not apply because of the location condition.

**Finding:** The What If results showed location-based targeting and exclusion behavior consistent with the policy design.

**Recommendation:** Maintain documented named locations, review exclusions regularly, and validate legitimate remote-access requirements before enforcement. Avoid relying on location as the only identity-security control.

### CA-004 — Require MFA for Admins

**Objective:** Require stronger authentication for privileged administrative access.

**Test evidence:** An administrative-role scenario was tested after assigning James Wilson the Application Administrator role; CA-004 applied and required multifactor authentication. Testing also demonstrated that users without a targeted administrative role did not receive the policy.

**Finding:** Role-based targeting functioned as intended during What If testing, and the policy produced an MFA requirement for a targeted administrator.

**Recommendation:** Protect all relevant privileged roles, maintain emergency-access exclusions, and prefer least-privilege/time-bound administration where available. Review role assignments before enabling enforcement.

### CA-005 — Require Compliant Device

**Objective:** Require a device that satisfies the configured compliance condition before access is granted to targeted resources.

**Test evidence:** What If testing showed CA-005 applying with the 'Require compliant device' grant control for an included scenario and not applying for an excluded user scenario.

**Finding:** The policy's inclusion/exclusion logic and compliant-device grant control were visible in the What If results.

**Recommendation:** Before enforcement, confirm device enrollment and compliance reporting are operational for the intended population. Keep narrowly justified exclusions and document an exception process for devices that cannot meet compliance requirements.

### CA-006 — Require MFA for Sign-in Risk

**Objective:** Apply MFA when Microsoft Entra ID evaluates a sign-in at the configured risk threshold.

**Test evidence:** A Medium sign-in-risk What If scenario caused CA-006 to apply and require multifactor authentication. A No risk scenario caused CA-006 not to apply because the sign-in-risk condition was not met.

**Finding:** Risk-based targeting responded differently to the tested Medium and No risk conditions as intended.

**Recommendation:** Keep the policy in Report-only until risk detections and user impact are reviewed. Define the organization's acceptable risk threshold and response process, then enforce with appropriate monitoring and incident-response procedures.

### CA-007 — Require Password Change for User Risk

**Objective:** Require remediation when an identity reaches the configured user-risk level.

**Test evidence:** A High user-risk What If scenario showed CA-007 applying with multifactor authentication/authentication-strength and password-change requirements. A No risk scenario showed the policy not applying because the user-risk condition was not met.

**Finding:** The policy differentiated a high-risk identity from a no-risk identity and surfaced the intended remediation controls.

**Recommendation:** Validate self-service password reset and MFA readiness before enforcement so affected users can complete remediation. Pair the policy with monitoring and investigation procedures for risky-user detections.

### CA-008 — Sign-in Frequency

**Objective:** Limit session lifetime by requiring targeted users to reauthenticate at a defined interval.

**Test evidence:** A positive What If scenario showed CA-008 applying with a 12-hour sign-in frequency session control. An excluded-user scenario showed CA-008 not applying because the user/group condition was not met.

**Finding:** The session-control policy applied to the targeted scenario and respected the tested user exclusion.

**Recommendation:** Balance the 12-hour reauthentication interval against user experience and resource sensitivity. Review exclusions and sign-in behavior before moving the policy from Report-only to enforcement.

## Cross-Policy Observations

- Report-only validation allows expected impact and policy logic to be reviewed before enforcement.
- Positive and negative scenarios provide complementary evidence of correct targeting.
- Exclusions should be narrowly scoped, documented, and periodically reviewed.
- Privileged identities require stronger authentication and disciplined role governance.
- Risk- and device-based controls depend on supporting identity, MFA, compliance, and remediation capabilities.

## Recommended Production Rollout

1. Confirm policy scope, dependencies, and exclusions.
2. Review Report-only and sign-in results for unexpected impact.
3. Validate emergency-access procedures.
4. Pilot enforcement with a limited population.
5. Monitor sign-ins, support impact, and security events.
6. Document approvals, exceptions, and rollback steps.

## Evidence

Supporting screenshots are organized under `Screenshots/01-Environment-Setup/` and `Screenshots/02-Conditional-Access-Policies/`, with a dedicated folder for each Conditional Access policy.

> This is a simulated portfolio lab. Production deployment would require organization-specific requirements, approvals, licensing, monitoring, and change-control procedures.

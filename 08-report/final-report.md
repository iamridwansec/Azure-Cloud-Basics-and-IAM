# Azure Cloud Basics and IAM — Final Security Assessment Report

**Project:** Azure Cloud Basics and IAM  
**Environment:** Microsoft Azure for Students  
**Assessment Type:** Cloud Security Configuration and Baseline Review  
**Status:** Completed with identified limitations

---

## 1. Executive Summary

This project involved configuring and reviewing foundational cloud security controls in a Microsoft Azure environment.

The assessment focused on identity and access management, privileged account security, activity monitoring, cost management, and storage protection.

Several security controls were successfully configured and validated, including Microsoft Entra Security Defaults, monthly budget alerts, HTTPS-only storage access, TLS 1.2, disabled anonymous Blob access, and disabled Shared Key authorization.

The assessment also identified areas requiring further attention, including individual MFA verification, custom role deployment, Activity Log event validation, and storage network configuration.

The project demonstrates a practical approach to cloud security: configure controls, verify their behavior, document evidence, and identify remaining risks.

## 2. Objectives

The main objectives were to:

- Secure the cloud account and privileged identity.
- Understand Azure identity and access management.
- Apply the principle of least privilege.
- Review subscription-level activity monitoring.
- Configure budget notifications.
- Protect Azure Storage against unauthorized access.
- Validate implemented controls and document security gaps.

## 3. Assessment Scope

The assessment covered the following areas:

| Area | Scope |
|---|---|
| Account security | Azure subscription and account configuration |
| Identity security | Microsoft Entra Security Defaults and security information |
| Access management | Azure RBAC and custom role definition drafts |
| Activity monitoring | Azure Activity Log and diagnostic settings |
| Cost management | Monthly budget and notification thresholds |
| Storage security | Storage account, container access, network rules, and transport security |
| Validation | Configuration review and unauthenticated access testing |

The assessment was limited to the Azure for Students environment and the resources created for this learning project.

No production systems or third-party environments were assessed.

## 4. Environment Overview

The project used:

- Microsoft Azure for Students subscription.
- Microsoft Entra ID Free.
- Azure Resource Manager and Azure RBAC.
- Azure Activity Log.
- Azure Cost Management.
- Azure Storage Account.
- Azure CLI and Microsoft Azure Portal.

### Storage Resource

| Property | Value |
|---|---|
| Resource group | `rg-cloud-iam-lab` |
| Storage account | `c1ph3rstorage26` |
| Region | Germany West Central |
| Account type | StorageV2 |
| Performance | Standard |
| Redundancy | Locally Redundant Storage (LRS) |
| Container | `security-evidence` |
| Container access | Private |

## 5. Security Controls Implemented

### 5.1 Account and Identity Security

- Confirmed the Azure subscription was active.
- Reviewed subscription-level role assignments.
- Enabled Microsoft Entra Security Defaults.
- Inspected the account's security information.

**Limitation:** Individual MFA registration and successful MFA completion were not independently verified.

### 5.2 Identity and Access Management

- Reviewed Azure RBAC assignments.
- Inspected built-in role definitions.
- Created local JSON drafts for a Storage Report Reader and Security Auditor Reader role.

**Limitation:** The custom role definitions were not deployed or assigned. The account retained the Owner role at subscription scope.

### 5.3 Activity Monitoring

- Reviewed Azure Activity Log.
- Inspected subscription diagnostic settings.
- Checked the available logging categories.

**Limitation:** No Activity Log events were returned during the review period. No diagnostic export destination was configured, and successful event-based validation remains outstanding.

### 5.4 Cost Management

Created a monthly Azure budget with the following configuration:

| Setting | Value |
|---|---|
| Budget name | `Azure-Student-Monthly-Budget` |
| Budget amount | $100 |
| Frequency | Monthly |
| Alert threshold 1 | 50% |
| Alert threshold 2 | 80% |
| Alert threshold 3 | 100% |
| Notifications | Email |

Budget alerts provide notifications and do not automatically stop resource deployment or spending.

### 5.5 Storage Protection

The storage account was configured and reviewed for transport security, authorization, and network restrictions.

Verified settings:

- HTTPS-only traffic enabled.
- Minimum TLS version set to TLS 1.2.
- Anonymous Blob access disabled.
- Shared Key access disabled.
- SFTP disabled.
- Private container created.

Additional observations:

- Hierarchical namespace is enabled.
- NFS v3 remains enabled.
- Public network access is enabled.
- The default network rule is Deny.
- Azure Services bypass is enabled.
- No explicit IP or virtual network rules were configured.

An unauthenticated request to list the private container returned HTTP 403. This confirms that the request was blocked, although the storage firewall may also have contributed to the result.

## 6. Validation Results

| Security area | Result |
|---|---|
| Azure subscription setup | PASS |
| Entra Security Defaults | PASS |
| Individual MFA verification | NOT VERIFIED |
| RBAC review | PASS |
| Custom role deployment | NOT VERIFIED |
| Activity Log review | PARTIAL |
| Budget and alerts | PASS |
| HTTPS-only storage | PASS |
| Minimum TLS 1.2 | PASS |
| Anonymous Blob access disabled | PASS |
| Shared Key access disabled | PASS |
| SFTP disabled | PASS |
| Storage network restrictions | PARTIAL |
| NFS v3 configuration review | REVIEW |

The detailed validation record is available in `../07-validation/validation.md`.

## 7. Findings and Recommendations

### Finding 1 — Individual MFA verification incomplete

**Observation:** Security Defaults are enabled, but individual MFA registration and successful challenge completion were not verified.

**Recommendation:** Register and test an appropriate MFA method.

### Finding 2 — Broad subscription permissions

**Observation:** The current account has the Owner role at subscription scope.

**Recommendation:** Use least-privilege roles for routine tasks and reserve Owner access for necessary administrative operations.

### Finding 3 — Custom roles not deployed

**Observation:** Custom role definitions exist as local JSON drafts but have not been deployed or tested.

**Recommendation:** Review their permissions, deploy them in a controlled environment, and verify their effective access.

### Finding 4 — Activity Log validation incomplete

**Observation:** No events were returned during the review period, and diagnostic export was not configured.

**Recommendation:** Generate a controlled administrative event and verify its appearance in Activity Log. Configure centralized export if required.

### Finding 5 — Storage network configuration requires review

**Observation:** Public network access is enabled, the default network rule is Deny, and Azure Services bypass is enabled. No explicit IP or virtual network rules are configured.

**Recommendation:** Review whether public endpoint access is necessary. Consider a private endpoint for workloads requiring private connectivity.

### Finding 6 — NFS v3 remains enabled

**Observation:** NFS v3 is enabled and could not be changed through the installed Azure CLI command interface.

**Recommendation:** Confirm whether NFS is required. If it is unnecessary, investigate the supported configuration path before using the account for a production workload.

## 8. Limitations

This project was completed in an Azure for Students environment.

The following limitations affected the assessment:

- Entra ID Free feature availability.
- Individual MFA completion was not independently verified.
- Custom roles were not deployed.
- Activity Log event collection was not fully validated.
- Diagnostic export was not configured.
- NFS v3 remained enabled.
- The storage account retained an enabled public endpoint with a default-deny network rule.

These limitations are documented to distinguish verified security controls from proposed improvements.

## 9. Conclusion

The Azure Cloud Basics and IAM project provided hands-on experience with foundational cloud security configuration and assessment.

It covered identity security, Azure RBAC, activity monitoring, cost management, and storage protection.

The assessment successfully implemented several important controls and identified additional improvements needed to strengthen the environment.

The project also reinforced the importance of validating configurations rather than assuming that a successful deployment automatically means a resource is secure.

## 10. Evidence and Project Documentation

Supporting documentation and evidence are organized throughout the repository:

- `01-account-setup/`
- `02-admin-security/`
- `03-iam-least-privilege/`
- `04-activity-log/`
- `05-billing-alerts/`
- `06-storage-protection/`
- `07-validation/`
- `images/`

All evidence should be reviewed with sensitive subscription identifiers, tenant identifiers, personal information, and other confidential values redacted before public distribution.

# Stage 7 — Validation and Security Review

## 1. Objective

This stage validates the Azure cloud security configuration completed in Stages 1–6.

The review distinguishes between:
- **PASS:** A setting or control was verified.
- **PARTIAL:** A control is configured, but has limitations or incomplete coverage.
- **NOT VERIFIED:** There is insufficient evidence to confirm the control.
- **REVIEW:** A configuration exists that requires further security consideration.

Validation is based on available Azure Portal evidence, Azure CLI output, and configuration tests.

## 2. Validation Summary

| Stage | Area | Result |
|---|---|---|
| 01 | Account Setup | PASS |
| 02 | Admin Security | PARTIAL |
| 03 | IAM and Least Privilege | PARTIAL |
| 04 | Activity Log | PARTIAL |
| 05 | Billing and Budget Alerts | PASS |
| 06 | Storage Protection | PARTIAL |

## 3. Stage-by-Stage Results

### Stage 1 — Account Setup

**Status: PASS**

Verified:
- Azure for Students subscription is active.
- Subscription is associated with the Default Directory.
- Current account has the Owner role at subscription scope.

Evidence:
- `images/01-account-setup-subscription.png`
- `images/01-account-setup-owner-rbac.png`

Security observation:

The Owner role provides broad administrative permissions. It was retained for this learning environment, but a production environment should use least-privilege role assignments.

### Stage 2 — Admin Security

**Status: PARTIAL**

Verified:
- Microsoft Entra Security Defaults are enabled.
- Security information page was reviewed.

Evidence:
- `images/02-admin-security-defaults.png`

Limitations:
- Individual MFA registration and successful MFA completion were not independently verified.
- The available security information showed email-based verification, which is not treated here as proof of a qualifying MFA method.

Recommendation:
- Complete registration of an appropriate MFA method and verify that the sign-in challenge works.

### Stage 3 — IAM and Least Privilege

**Status: PARTIAL**

Verified:
- Subscription role assignments were inspected.
- Built-in role definitions were reviewed.
- Custom role definition JSON drafts were created and validated locally.

Evidence:
- `images/03-iam-my-access.png`
- `images/03-iam-role-assignments.png`
- `images/03-iam-role-definitions.png`

Custom role drafts:
- `roles/storage-report-reader.json`
- `roles/security-auditor-reader.json`

Limitations:
- The custom roles are local JSON drafts; they were not deployed or assigned in Azure.
- The current account retains the Owner role.
- The proposed security auditor role contains broad read permissions and requires further review before production use.

Recommendation:
- Deploy and test narrowly scoped custom roles in a controlled environment.
- Replace broad administrative access with task-specific roles where practical.

### Stage 4 — Activity Log

**Status: PARTIAL**

Verified:
- Azure Activity Log was inspected.
- The last seven days were queried, but no events were returned during the review.
- Subscription diagnostic settings were inspected.
- No diagnostic export setting was configured.

Evidence:
- `images/04-activity-log-diagnostic-settings.png`

Limitations:
- No successful event-based validation was completed.
- The Azure CLI Activity Log query encountered a local CLI dependency error.
- Diagnostic export to an external destination was not configured.

Recommendation:
- Generate a controlled administrative activity and confirm that it appears in the Activity Log.
- Configure diagnostic export if centralized retention or monitoring is required.

### Stage 5 — Billing and Budget Alerts

**Status: PASS**

Verified:
- A monthly Azure budget was created.
- Budget name: `Azure-Student-Monthly-Budget`
- Budget amount: `$100`
- Notification thresholds: 50%, 80%, and 100%.
- Email notifications were configured.

Evidence:
- `images/05-billing-budget-alerts.png`

Security and cost observation:

Budget alerts provide spending notifications. They do not automatically stop resources or prevent additional charges.

### Stage 6 — Storage Protection

**Status: PARTIAL**

Storage account: `c1ph3rstorage26`

Resource group: `rg-cloud-iam-lab`

Verified configuration:

| Setting | Observed value | Result |
|---|---|---|
| HTTPS-only traffic | Enabled | PASS |
| Minimum TLS | TLS 1.2 | PASS |
| Anonymous Blob access | Disabled | PASS |
| Shared Key access | Disabled | PASS |
| SFTP | Disabled | PASS |
| Hierarchical namespace (HNS) | Enabled | REVIEW |
| NFS v3 | Enabled | REVIEW |
| Public network access | Enabled | REVIEW |
| Default network rule | Deny | PASS |
| IP network rules | None configured | REVIEW |
| Virtual network rules | None configured | REVIEW |
| Trusted Azure services bypass | Enabled | REVIEW |

Additional validation:
- The `security-evidence` container was configured with private anonymous access.
- An unauthenticated container-list request returned HTTP 403.
- The 403 response confirms that the request was blocked, but does not independently prove that the container's privacy setting caused the denial; the storage firewall also restricts network access.

Evidence:
- `images/06-storage-deployment.png`
- `images/06-storage-configuration.png`
- `images/06-storage-private-container.png`

Limitations and observations:
- Public network access remains enabled.
- The default network action is Deny, but no explicit IP or virtual network rules are configured.
- The Azure Services bypass is enabled.
- HNS and NFS v3 remain enabled. These settings are documented as observed, not as disabled.
- SFTP was successfully disabled.
- NFS v3 could not be changed using the installed Azure CLI command interface.

Recommendation:
- Review whether HNS and NFS v3 are required for the intended workload.
- Consider disabling public network access and using a private endpoint if the workload requires private-only connectivity.
- If public network access is necessary, define and test the specific permitted network rules.

## 4. Overall Security Assessment

The lab demonstrates several important cloud security controls:

- Subscription and identity configuration review.
- Entra Security Defaults.
- Azure RBAC inspection.
- Activity Log review.
- Budget monitoring.
- Storage encryption and transport security settings.
- Anonymous Blob access protection.
- Shared Key access restriction.
- Storage network access restrictions.

However, some controls remain incomplete or require further validation.

The most important remaining improvements are:

1. Verify an appropriate MFA method.
2. Deploy and test least-privilege custom roles.
3. Confirm Activity Log event collection.
4. Review storage network exposure and trusted-service exceptions.
5. Review whether NFS v3 and hierarchical namespace are necessary.

## 5. Conclusion

The Azure Cloud Basics and IAM lab successfully established and tested several foundational cloud security controls using an Azure for Students subscription.

The project also identified limitations in identity verification, custom role deployment, activity monitoring, and storage configuration.

Rather than treating every configuration as fully secure, this validation records the observed settings, identifies incomplete controls, and provides recommendations for further hardening.

The result is a documented practical exercise in cloud account security, identity and access management, monitoring, cost control, and storage protection.

## 6. References

- [Microsoft Learn — Set the default public network access rule for Azure Storage](https://learn.microsoft.com/en-us/azure/storage/common/storage-network-security-set-default-access)
- [Microsoft Learn — Secure an Azure Storage account](https://learn.microsoft.com/en-us/azure/storage/common/storage-security)

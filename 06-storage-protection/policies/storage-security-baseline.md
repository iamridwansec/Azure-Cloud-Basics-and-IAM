# Azure Storage Security Policy Baseline

## Purpose

Define the security controls applied to the Azure Storage Account during Stage 6 of the Cloud Basics and IAM assessment.

## Required Security Controls

| Control | Expected configuration |
|---|---|
| Anonymous Blob access | Disabled |
| Shared key authorization | Disabled |
| Secure transfer | Enabled |
| Minimum TLS | TLS 1.2 |
| Container anonymous access | Private |
| Data recovery | Soft delete enabled |

## Azure Baseline Security Mode

The Azure subscription's Azure Baseline Security Mode initiative includes relevant built-in policies:

- Storage account public access should be disallowed.
- Storage accounts should prevent shared key access.
- Storage accounts should have the specified minimum TLS version.
- Secure transfer to storage accounts should be enabled.

These policies provide a reference baseline for assessing storage security.

## Implementation Status

The storage security settings were configured during Storage Account deployment.

This file documents the assessment baseline. It is not a custom Azure Policy definition and does not represent a separate policy deployment.

## Security Principle

Storage resources should use authenticated access, encrypted communication, restricted network exposure, and recovery controls to reduce the risk of unauthorized data access or loss.

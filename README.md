

![image](https://github.com/user-attachments/assets/1399ce62-19ce-4054-b451-ac5177a123bd)

# Terraform Infra Divergence Management Understanding

|**Author**        | **created on**       | **Version** |**Last edited on**| **Review Level**   | **Reviewer**      |
|---------------|------------|---------|--------|--------|----------------------|
| Anitha Annem  | July 01  | v1.0| July 02   | Pre-Reviewer   | Priyanshu            |
| Anitha Annem  |  |  |   | L0             | Khushi Malhothra    |
| Anitha Annem  |     |      |         | L1             | Mukul Joshi       |
| Anitha Annem  |     |      |         | L2             | piyush Upadhyay      |

# Table of Contents

- [Introduction](#introduction)
- [What is Infrastructure Divergence?](#what-is-infrastructure-divergence)
- [Why manage infrastructure divergence?](#why-manage-infrastructure-divergence)
- [Causes of Infrastructure Divergence](#causes-of-infrastructure-divergence)
- [Detection Methods](#detection-methods)
- [Management Strategies](#management-strategies)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [References](#references)


# Introduction

As organizations increasingly adopt Infrastructure as Code (IaC) practices, managing infrastructure through declarative configurations (such as Terraform) has become essential for ensuring consistency, repeatability, and scalability.  
This document explains what infrastructure divergence is, its common causes, how to detect it, and best practices to manage and prevent it effectively. By following these strategies, teams can maintain a secure, stable, and predictable infrastructure environment.


# What is Infrastructure Divergence?

Infrastructure divergence refers to discrepancies between:

- **Terraform configuration (desired state)**: The code that defines what your infrastructure should look like.
- **Terraform state file (known state)**: The snapshot that Terraform uses to track the infrastructure it manages.
- **Actual deployed infrastructure (real state)**: The current, real-world state of your resources in the cloud or data center.

When these are out of sync, it can lead to unexpected behavior, security issues, and operational problems.

# Why manage infrastructure divergence?

Managing infrastructure divergence is important because discrepancies can lead to:

- **Unexpected behavior**: Systems may not work as intended if configurations are out of sync.
- **Security risks**: Untracked changes can introduce vulnerabilities or expose sensitive resources.
- **Operational problems**: Inconsistent environments can cause outages, deployment failures, and troubleshooting delays.
- **Increased maintenance overhead**: Manual fixes and undocumented changes make the infrastructure harder to maintain and scale.

# Causes of Infrastructure Divergence

| Cause                   | Explanation                                                                                   |
|-------------------------|-----------------------------------------------------------------------------------------------|
| Manual changes (drift)  | Direct changes to infrastructure outside Terraform (e.g., console updates, CLI edits).       |
| Out-of-band tools       | Other tools or scripts modify infrastructure resources without updating Terraform state.     |
| Failed or partial applies | Errors during `terraform apply` can leave resources partially updated.                     |
| State file inconsistencies | Corruption or loss of state files, or using outdated state copies.                       |
| Resource lifecycle changes | Changes in resource behavior by the cloud provider or API changes.                       |


# Detection Methods

### 1. Terraform Plan

Running `terraform plan` compares the desired configuration and current infrastructure state (according to the state file).

It helps identify unexpected changes that will be applied.

---

### 2. Terraform Refresh

`terraform refresh` updates the state file with the actual current state from the provider.

This reveals drift before planning or applying changes.

---

### 3. `terraform plan -detailed-exitcode`

- Exit code 0: No changes.
- Exit code 2: Changes present (possible drift).

This can be used in CI pipelines to fail builds on unexpected drift.

---

### 4. Terraform Drift Detection Tools

Tools such as [Driftctl](https://github.com/snyk/driftctl) analyze cloud resources directly and detect unmanaged or drifted resources.

Cloud provider tools (like AWS Config or Azure Resource Graph) can also help detect configuration drifts.


# Management Strategies

### 1. Enforce Infrastructure as Code (IaC) only

**Description:**  
Prohibit manual changes through consoles or other tools. All changes must go through Terraform.

**How to enforce:**  
Use strict IAM permissions and clear team policies.

---

### 2. Regular drift detection checks

**Description:**  
Schedule periodic `terraform plan` runs or use automated drift detection tools in your CI/CD pipeline.

**Benefits:**  
Identify divergence early before it causes issues.

---

### 3. Automated reconciliation

**Description:**  
When drift is detected, run `terraform apply` to reconcile the actual state back to the desired state.

**Caution:**  
Always review plans carefully before applying to avoid unintentional destruction.

---

### 4. Use state locking and controlled access

**Description:**  
Enable state locking (e.g., with remote backends like S3 + DynamoDB for AWS) to prevent concurrent changes.

**Benefits:**  
Avoids conflicts and unexpected overwrites.

---

### 5. Monitor and remove unmanaged resources

**Description:**  
Regularly audit and clean up resources that are not managed by Terraform.

**Tools:**  
DriftCTL, cloud resource inventory tools.

---

### 6. Version control and change reviews

**Description:**  
Store Terraform code in version control (e.g., Git) and use pull requests with peer reviews before merging.

**Benefits:**  
Ensures all changes are reviewed, documented, and approved.

---

### 7. Implement policy-as-code

**Description:**  
Use tools like Sentinel (Terraform Cloud), OPA (Open Policy Agent), or custom scripts to enforce standards.

**Benefits:**  
Prevent changes that do not comply with security or operational policies.

# Conclusion

Infrastructure divergence is a critical challenge in cloud-native and IaC-driven environments. By understanding its causes, detecting it early, and applying effective management strategies, teams can maintain secure, stable, and predictable infrastructure.  

Proactive divergence management strengthens operational resilience, reduces security risks, and ensures that infrastructure remains a reliable foundation for delivering business value.

# Contact Information 
| Name       | Email Address                |
|------------|------------------------------|
| Anitha     |anitha.annem.snaatak@mygurukulam.co|

# References
| **Link** | **Description** |
|------------------------------------------------------|------------------|
| [driftctl](https://docs.driftctl.com/0.40.0)| documentation of driftctl      |





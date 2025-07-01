![image](https://github.com/user-attachments/assets/ea6490fa-c14b-4cb9-8f39-17a775a812e7)

# Ansible Unit Test Documentation


|**Author**        | **created on**       | **Version** |**Last edited on**| **Review Level**   | **Reviewer**      |
|---------------|------------|---------|--------|--------|----------------------|
| Anitha Annem  | Jul 01  | v1.0|  Jul 02    | Pre-Reviewer   | Priyanshu            |
| Anitha Annem  |  |  |   | L0             | Khushi Malhothra    |
| Anitha Annem  |     |      |         | L1             | Mukul Joshi       |
| Anitha Annem  |     |      |         | L2             | piyush Upadhyay      |

# Introduction
This document provides a detailed overview of Ansible unit testing, focusing on its purpose, the tools used (primarily Molecule), setup procedures, and practical usage examples. By adopting unit testing for Ansible roles and playbooks, teams can improve automation reliability, catch errors early, and maintain high-quality infrastructure code. 

# Purpose of Ansible Unit Testing

Ansible unit testing ensures that individual roles, tasks, and playbooks work as intended before deployment. The primary goals are to:

- **Validate logic in smaller units (roles or tasks)**
- **Catch configuration errors early**
- **Improve code quality and maintainability**
- **Enable safe refactoring and faster feedback**

# Why Ansible Unit Test?

| Reason             | Description                                                                                      |
|--------------------|--------------------------------------------------------------------------------------------------|
| Catch errors early | Find configuration mistakes before they affect production systems.                               |
| Ensure code quality| Validate that each role or task does exactly what it should.                                     |
| Support refactoring| Safely update or improve code with confidence that existing functionality still works.          |
| Faster feedback loop | Quickly verify changes during development instead of waiting for full deployments.            |
| Improve reliability | Reduce the risk of deployment failures and ensure more stable infrastructure.                  |


# Tools for Ansible Unit Testing

### Molecule

Molecule is the most popular tool for testing Ansible roles. It provides:

- A framework for writing unit and integration tests
- Support for multiple drivers (Docker, Podman, Vagrant, etc.)
- Easy linting, syntax checks, and idempotence tests

### Other tools used alongside Molecule

- **Testinfra**: Used for verifying the state of provisioned resources
- **Ansible Lint**: For static code analysis

# Setup Steps

### 1. Install required tools

```bash
pip install molecule ansible-lint docker
```
Note: Replace docker with another driver if needed (e.g., podman).

### 2. Initialize a Molecule scenario
Inside your Ansible role directory:
```
molecule init scenario --scenario-name default --driver-name docker
```

### 3. Configure molecule.yml
Edit molecule/default/molecule.yml to define your test environment, instances, and driver settings.

### 4. Write verify tests
Under molecule/default/tests/test_default.py, write verification tests using Testinfra. Example:

```
def test_hosts_file(host):
    f = host.file("/etc/hosts")
    assert f.exists
    assert f.user == "root"
```

# Usage Example

### Run all tests

```bash
molecule test
```
This runs the complete test sequence: lint, create, converge, verify, and destroy.

### Run specific stages
```
molecule converge
molecule verify
molecule destroy
```

# Conclusion
Ansible unit testing, especially with Molecule, helps ensure roles are reliable and maintainable by testing them in isolated environments before production. This practice reduces deployment risks and improves automation quality.

# Contact Information 
| Name       | Email Address                |
|------------|------------------------------|
| Anitha     |anitha.annem.snaatak@mygurukulam.co|

# References
| **Link** | **Description** |
|------------------------------------------------------|------------------|
| [Molcule documentation](https://molecule.readthedocs.io/)| Documentation on Molecule      |

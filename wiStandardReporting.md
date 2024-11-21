# Setting RBAC for Aria Operations Dashboard

## Table of Contents
- [Standard Reporting](#Standard-Reporting)
  - [Table of Contents](#table-of-contents)
  - [List of Changes](#list-of-changes)
  - [Introduction](#introduction)
    - [Purpose](#Scope)
   
## List of Changes

| Version | Date       | Author       | Issue    | Changes           |
|---------|------------|--------------|----------|-------------------|
| 0.1     | 22.11.2024 | Rachel Beulah | VCS-14360| Document creation |

## Introduction

### Purpose

Configuring RBAC for vROps dashboards involves integrating Active Directory, defining a scope and a role, and assigning them to a specific user group or user, ensuring they can only view or access dashboards linked to their assigned scope and role upon logging into vROps.

## Integrating with Active Directory

Integrating Active Directory with vROps simplifies user management by allowing centralized authentication and role-based access control, enabling users to log in with their credentials while ensuring secure and scalable access.

The below playbook will integrate Active Directory with vROps using API,
```markdown
ansible-playbook createScope.yml
```

![image](/workInstructions/images/wiStandardReporting/ADintegration.png)


## Creation of Resource group


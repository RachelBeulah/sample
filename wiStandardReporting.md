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

Creating three resource groups in AD: `vROps-readonly, vROps-admin, and vROps-dashboard` enables efficient management of role-based access in vROps by assigning specific permissions, such as read-only, admin rights, or dashboard access, to users securely and effectively.

The below playbook will creates resource groups within a specified Active Directory using PowerShell,
```markdown
ansible-playbook createADresourcegroup.yml
```

![image](/workInstructions/images/wiStandardReporting/ADresourcegroup.png)


## Creation of Scope

Scope refers to a defined set of resources or objects, such as VMs or clusters, that restricts actions, dashboards, or policies to only the relevant resources for specific users or groups.

The below playbook will creates scopes in vROps using APIs,
```markdown
ansible-playbook createScope.yml
```
- A scope will be created in vROps to include all vCenters except VCS001 (management), covering potential customer workload vCenters such as VCS002 to VCS014, as well as the NSX-T inventory, including NSX Groups.
- Two scopes will be created in vROps: one for all vCenters except VCS001 (management), covering potential customer workload vCenters such as VCS002 to VCS014, and another for the NSX-T inventory, which includes NSX Groups.

![image](/workInstructions/images/wiStandardReporting/scope.png)
![image](/workInstructions/images/wiStandardReporting/scope.png)


## Creation of Role

 Role defines a set of permissions that control user access to specific features and data. Roles determine what actions a user can perform, such as viewing, configuring, or managing resources within vROps.

 - A role will be created to provide visibility and access to 'Dashboards' and 'Reports' under 'Visualize,' while excluding areas such as 'Administration' and 'Automation Central.

![image](/workInstructions/images/wiStandardReporting/role.png)



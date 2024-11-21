# Setting RBAC for Aria Operations Dashboard

## Table of Contents
- [Setting RBAC for Aria Operations Dashboard](#Setting-RBAC-for-Aria-Operations-Dashboard)
  - [Table of Contents](#table-of-contents)
  - [List of Changes](#list-of-changes)
  - [Introduction](#introduction)
    - [Purpose](#Purpose)
  - [Integrating with Active Directory](#Integrating-with-Active-Directory)
  - [Creation of Resource group](#Creation-of-Resource-group)
  - [Creation-of-Scope](#Creation-of-Scope)
  - [Creation of Role](#Creation-of-Role)
  - [Creation of Custom groups](#Creation-of-Custom-groups)
  - [Assigning RBAC to a user group/user](#Assigning-RBAC-to-a-user-group/user)
  - [Synchronize User group](#Synchronize-User-group)
  - [Update Service Account Password](#Update-Service-Account-Password)
- [Summary](#Summary)
   
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
- Creating a scope in vROps will include all vCenters except VCS001 (management), covering potential customer workload vCenters such as VCS002 to VCS014, as well as the NSX-T inventory, including NSX Groups.
- Creating two scopes in vROps: one for all vCenters except VCS001 (management), covering potential customer workload vCenters such as VCS002 to VCS014, and another for the NSX-T inventory, which includes NSX Groups.

![image](/workInstructions/images/wiStandardReporting/scope.png)

![image](/workInstructions/images/wiStandardReporting/scope.png)


## Creation of Role

 Role defines a set of permissions that control user access to specific features and data. Roles determine what actions a user can perform, such as viewing, configuring, or managing resources within vROps.

 - Creating a role that will provide visibility and access to 'Dashboards' and 'Reports' under 'Visualize,' while excluding areas such as 'Administration' and 'Automation Central.

![image](/workInstructions/images/wiStandardReporting/role.png)


## Creation of Custom groups

Custom groups are user-defined collections of resources (like VMs or hosts) that allow for easier management and monitoring. They enable grouping based on specific criteria or organizational needs, enhancing resource visibility and reporting.

The below playbook will creates a custom group in vROps using API,
```markdown
ansible-playbook createCustomGroup.yml
```
- Creating custom groups with vCenter tags, like resource pool, cluster, and datacenter, and using them for RBAC in dashboards provides better access control and resource management.

![image](/workInstructions/images/wiStandardReporting/customgroups.png)


## Assigning RBAC to a user group/user

Assigning RBAC roles and scopes to a user group/user in vROps for dashboards involves granting specific permissions through roles and defining which dashboards they can access through scopes. This ensures that users or user groups can view or manage specific dashboards based on their assigned roles, maintaining secure and organized access to dashboard data.

The below playbooks will assign permissions to specific user groups/user, including roles and scopes,
```markdown
ansible-playbook appendAccesstoUsergrp.yml
```
```markdown
ansible-playbook appendAccesstoUser.yml
```
![image](/workInstructions/images/wiStandardReporting/rbacuser.png)

![image](/workInstructions/images/wiStandardReporting/rbacusergrp.png)


## Synchronize User group

Synchronizing user groups in the "Authorized Sources" ensures that the user groups from external authentication systems, such as Active Directory, are updated and available in vROps for role-based access control and permissions management.

The below playbooks will synchronizes user groups in the "Authorized Sources" tab using the vROps API,
```markdown
ansible-playbook synchronizeUsergroup.yml
```
![image](/workInstructions/images/wiStandardReporting/syncusergrp.png)


## Update Service Account Password

Updating the service account password for Active Directory, which is used to integrate AD with vROps, involves resetting the password in Active Directory and then updating the credentials in vROps to ensure continued synchronization and authentication.

The below playbooks will update the service account password for active directory using API,
```markdown
ansible-playbook updateServiceAccountPassword.yml
```
![image](/workInstructions/images/wiStandardReporting/updatepassword.png)


# Summary

This process integrates Active Directory (AD) with vROps to enable role-based access control (RBAC) for dashboards. It involves creating resource groups, scopes, and roles to manage permissions, followed by assigning RBAC to user groups and synchronizing them with vROps. The goal is to ensure users can only access the dashboards they are authorized for, securing access and simplifying management.


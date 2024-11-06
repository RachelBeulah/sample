# External Reports vROps

## Table of Contents
- [External Reports vROps](#External-Reports-vROps)
  - [Table of Contents](#table-of-contents)
  - [List of Changes](#list-of-changes)
  - [Introduction](#introduction)
    - [Purpose](#Scope)
  - [Automation](#Automation)
    - [Tasks](#Tasks)
      - [1. Collect all reports in Ansible server](#Collect-all-reports-in-Ansible-server)
      - [2. Collect only latest reports](#Collect-only-latest-reports)
      - [3. Convert to html file](#Convert-to-html-file)
      - [4. Copy Reports to Webserver](#Copy-Reports-to-Webserver)
      - [5. Host in Webserver](#Host-in-Webserver)
      - [6. Backup Reports](#Backup-Reports)
   
## List of Changes

| Version | Date       | Author       | Issue    | Changes           |
|---------|------------|--------------|----------|-------------------|
| 0.1     | 06.11.2024 | Rachel Beulah | VCS-14257| Document creation |

## Introduction

### Purpose

Collect all external reports, such as RV Tools, Nessus, Patching, etc., host them on a web server, and display them as a dashboard inside vROps using the Text Widget option.

## Automation

The following playbook will collect the latest reports from external tools such as RV Tools, Nessus, Patching, etc., onto the Ansible server, convert them to HTML files, rename them without timestamps, copy the latest reports to the web server, change the file permissions, and host them on the web server:

```markdown
ansible-playbook externalReportsVrops.yml
```

 - This playbook consists of several tasks that work together to collect and host the reports on the web server, which will be explained in detail below.

### Tasks

#### 1. Collect all reports in Ansible server

- The ```gatherReports.yml``` task will collect externally generated reports (Like RV tools, Nessus, Patching etc..) onto the Ansible server in the path ```/opt/reports/dhcReports```.

  ![image](/workInstructions/images/wiExternalReportsvROps/CollectReport.png)

#### 2. Collect only latest reports

 - The ```gatherReports.yml``` task collects reports with timestamps, including older reports, rather than only the latest ones. To address this, the ```collectLatestReport.yml``` task uses a shell script to retrieve only the most recent reports based on timestamp, renames the files to remove timestamps, and copies them to the ```/opt/reports/vROps``` directory.

   ![image](/workInstructions/images/wiExternalReportsvROps/LatestReport.png)

#### 3. Convert to html file

 - The ```convertReporttoHtml.yml``` task will check for different file format such as csv, xml, excel and convert them into html file using python script.

    ![image](/workInstructions/images/wiExternalReportsvROps/ConvertHtml.png)

#### 4. Copy Reports to Webserver

 - The ```cpReportstoWebserver.yml``` task will copy the latest reports from ```/opt/reports/vROps``` directory to Webserver ```/home/next/Reports```.
 - If any files already exist in the ```/home/next/Reports``` directory, they will be moved to a backup directory ```/home/next/Backup/Backup-<timestamp>``` before copying the latest reports into the directory.
 - Set the permissions of the reports to ```0777``` before hosting them on the web server.
   
    ![image](/workInstructions/images/wiExternalReportsvROps/Webserver.png)
   
    ![image](/workInstructions/images/wiExternalReportsvROps/ReportWebserver.png)
   
    ![image](/workInstructions/images/wiExternalReportsvROps/BackupWebserver.png)

#### 5. Host in Webserver

   - The ```hostReportinWebserver.yml``` task will host the reports in the webserver's ```/var/www/html``` directory and restart the Nginx service.

     ![image](/workInstructions/images/wiExternalReportsvROps/hostWebserver.png)
     
     ![image](/workInstructions/images/wiExternalReportsvROps/SampleNessusReport.png)

#### 6. Backup Reports

- The ```reportsBackup.yml``` task will create a backup of the reports located in ```/opt/reports/vROps``` and store them in ```/backup/dhc-report/Backup<timestamp>```.

     ![image](/workInstructions/images/wiExternalReportsvROps/backupAnsible.png)
     

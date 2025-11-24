# Assignment – 02

> **Topics Covered:** User Authentication & Authorization  
> **Objective:** Implement role-based access control (RBAC) for multiple teams

---

##  Table of Contents
- [Overview](#overview)
- [Part 1 – Role-Based Job and Access Setup](#part-1--role-based-job-and-access-setup)
  - [1️ Create Dummy Jobs](#1️-create-dummy-jobs)
  - [2️ Create Views](#2️-create-views)
  - [3️ Create Users](#3️-create-users)
  - [4️ Configure Authorization Strategy](#4️-configure-authorization-strategy)
  - [5️ Assign Roles and Permissions](#5️-assign-roles-and-permissions)


---

## Overview
An organization with **three teams**:
- **Developers**
- **Testers**
- **DevOps Engineers**

Each team has its own Jenkins jobs and restricted access levels.  
An `admin-1` user will have **full control** and **Google SSO** enabled.

---

## Part 1 – Role-Based Job and Access Setup

### 1. Create Dummy Jobs
Create nine Freestyle jobs that simply print the job name and build number.


| Team | Jobs |
|------|------|
| Developer | dev-1, dev-2, dev-3 |
| Testing | test-1, test-2, test-3 |
| DevOps | devops-1, devops-2, devops-3 |



### for eg,
    echo "Job: $JOB_NAME"
    echo "Build: $BUILD_NUMBER"

<img width="1302" height="267" alt="image" src="https://github.com/user-attachments/assets/cb41e132-0359-4c98-902b-b1c236025475" />
<img width="1297" height="130" alt="image" src="https://github.com/user-attachments/assets/e4a7d24c-315c-4965-bd57-45e2400b36b7" />

### 2. Create Team-Based Views
Go to:
Dashboard → + New View → List View

  | View Name | Include Jobs |
  |-----------|--------------|  
  | Developer View |	dev-* | 
  | Testing View |	test-* |
  | DevOps View |	devops-* |

**dev view**
<img width="1476" height="361" alt="image" src="https://github.com/user-attachments/assets/1deb70b3-9d0e-4d4d-9a60-13184423c5b0" />

**devops view**
<img width="1475" height="382" alt="image" src="https://github.com/user-attachments/assets/4faca276-f381-451a-8118-cfa9ddf75cbb" />

**test view**
<img width="1508" height="387" alt="image" src="https://github.com/user-attachments/assets/bdf7d788-a374-4273-980b-fc755bf9bef0" />

### 3. Create Users
Manage Jenkins → Security → Manage Users → Create User

|Team	| Users |
|-----|-------|
| Developer	| developer-1, developer-2 |
| Testing	| testing-1, testing-2 |
| DevOps	| devops-1, devops-2 |
| Admin	| admin-1 |
  

### 4: Configure Authorization Strategy
Install plugin:
Role-based Authorization Strategy
Then go to:
Manage Jenkins → Configure Global Security → Authorization → Role-Based Strategy
Save → Manage and Assign Roles → Manage Roles

<img width="940" height="482" alt="image" src="https://github.com/user-attachments/assets/42d39169-fe80-4083-8d0c-116f37ecc169" />

### 5: Define Roles and Permissions
  | Role |	Pattern (Regex) |	Permissions|
  |------|------------------|------------|
  |dev-jobs |	^dev-.* |	Build, Read, Configure, Workspace |
  |test-jobs | ^test-.*	| Build, Read, Configure, Workspace |
  |devops-jobs | ^devops-.* |	Build, Read, Configure, Workspace |
  
Administer access is given to Admin, while read access is given to dev, testa nd devops user
<img width="1761" height="532" alt="image" src="https://github.com/user-attachments/assets/0442ec52-98d4-4484-bee2-57acfc605f5d" />

### 6: Assign Users to Roles
  | User	| Global Role |	Project Roles|
  |-------|-------------|--------------|
  |developer-1	| developer	| dev-jobs |
  |developer-2	| developer	| dev-jobs |
  |testing-1	| testing |	test-jobs, dev-jobs (view) |
  |testing-2	| testing	| test-jobs, dev-jobs (view) |
  |devops-1	| devops | devops-jobs, dev-jobs, test-jobs |
  |devops-2	| devops | devops-jobs, dev-jobs, test-jobs |
  |admin-1	| admin	| all |
  
admin --> has admin role  
developer has --> devloper role  
tester has --> terster role  
devops has-- devops role  

<img width="902" height="727" alt="image" src="https://github.com/user-attachments/assets/2d687b24-fc3a-4211-a759-ddedd125200b" />




<img width="576" height="471" alt="image" src="https://github.com/user-attachments/assets/24c35684-d5d5-42c5-b540-fb8e4d836f98" />



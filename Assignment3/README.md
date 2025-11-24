#  Assignment 3 


## Objective
> Perform **Continuous Integration (CI) checks** on three API repositories using Jenkins.  
> Implement reporting, artifact management, and failure notifications.


## Repositories Used

| Language | Repository Link | Description |
|-----------|----------------|--------------|
| **Python** | [attendance-api](https://github.com/OT-MICROSERVICES/attendance-api) | API for attendance management |
| **GoLang** | [employee-api](https://github.com/OT-MICROSERVICES/employee-api) | API for employee services |
| **Java** | [spring3hibernate](https://github.com/opstree/spring3hibernate.git) | Java project using Spring and Hibernate |



##  Prerequisites

✅ Jenkins installed and configured  
✅ Required plugins:
-  **Git Plugin**
-  **JUnit Plugin**
-  **Cobertura / JaCoCo Plugin**
-  **Email Extension Plugin**
-  **OWASP Dependency-Check Plugin**
-  **Slack Notification Plugin**

✅ GitHub credentials added to Jenkins  
✅ Build tools installed (Python, Go, Java + Maven)


##  Job Configuration Steps

##  Step 1: Configure Source Code Management (SCM)

- Choose **Git**  
- Add repository URL 
<img width="1287" height="576" alt="image" src="https://github.com/user-attachments/assets/89e0e9ac-7c43-46f7-a586-1e341db0e8a1" />


##  Step 2: Add Build Steps

###  Python (attendance-api)

#### Execute Shell
<img width="1217" height="797" alt="image" src="https://github.com/user-attachments/assets/562e66fd-ab60-47b7-adfe-6912d9410aa6" />


#### Archive the artifacts + Publish HTML reports
<img width="1533" height="800" alt="image" src="https://github.com/user-attachments/assets/9f5f06f7-9544-4b93-8d6e-c369ac24edc0" />


#### Output
<img width="1387" height="650" alt="image" src="https://github.com/user-attachments/assets/c33163f2-64be-4ede-bd7d-a5f967d3b6bc" />



###  GoLang (employee-api)
#### Git clone

<img width="1451" height="690" alt="image" src="https://github.com/user-attachments/assets/f018628e-7e69-4216-b180-56d38a2df5a1" />

#### Execute shell
<img width="1312" height="778" alt="image" src="https://github.com/user-attachments/assets/013e4206-32a8-4cb8-83fe-d87347c90264" />

#### Artifacts 
<img width="1425" height="761" alt="image" src="https://github.com/user-attachments/assets/69a778ae-2217-4f83-9fd1-cb2c06b7fa8c" />

#### Output
<img width="1357" height="838" alt="image" src="https://github.com/user-attachments/assets/877032ef-f3ed-417d-acab-e6c78047f9b5" />






###  Java (spring3hibernate)
#### Git clone
<img width="1341" height="690" alt="image" src="https://github.com/user-attachments/assets/150786fb-e589-422f-8800-406fcb7aaf29" />

#### Execute shell
<img width="1205" height="717" alt="image" src="https://github.com/user-attachments/assets/01298244-d5b6-4708-b877-3c2e918d6edf" />

#### Artifacts
<img width="1362" height="741" alt="image" src="https://github.com/user-attachments/assets/a68e6278-34cb-40e9-a138-59248f1d3388" />

#### Output
<img width="1183" height="842" alt="image" src="https://github.com/user-attachments/assets/00d86ed6-524a-42bb-9ee6-ec97fac04c5d" />




##  Expected Output

- Three separate Jenkins jobs:
  - **attendance-api** → Python  
  - **employee-api** → GoLang  
  - **spring3hibernate** → Java  
- Each job performs automated CI checks on every build.  
- Reports (test, coverage, dependency) viewable within Jenkins.  
- Artifacts stored locally or remotely (based on configuration).  
- Failure notifications triggered via Slack or Email.  

---


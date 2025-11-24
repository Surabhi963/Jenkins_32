# Assignment 5

> **Goal:**  
> Create a **Declarative Jenkins CI Pipeline** for a **Java-based project**, with parallel stages for code stability, quality, and coverage, including artifact publication, approval gates, and notifications.

---

##  Objective
Design and implement a **declarative CI pipeline** that performs:
1. **Code Checkout**
2. **Parallel Stages:**
   - Code Stability
   - Code Quality Analysis
   - Code Coverage
3. **Report Generation**
4. **Artifact Publishing**
5. **Slack and Email Notifications**
6. **Manual Approval Stage before Publishing**
7. **Conditional Stage Execution** (skip scans)
8. *(Optional)* Implement the same using **Scripted Pipeline**

---

<details>
<summary> <b> Jenkins Prerequisites</b></summary>

###  Plugins Required
| Plugin | Purpose |
|---------|----------|
| Git Plugin | Source code checkout |
| Pipeline Plugin | Declarative/Scripted pipeline support |
| Maven Integration | Java build & test |
| JUnit Plugin | Publish test reports |
| JaCoCo Plugin | Coverage reports |
| SonarQube Plugin | Code quality analysis |
| Slack Notification Plugin | Slack alerts |
| Email Extension Plugin | Email alerts |
| Build Timeout Plugin | Timeout control |

###  Configure Global Tools
- **JDK** → Java 11 or later  
- **Maven** → Configured under `Global Tool Configuration`  
- **SonarQube** → Add under `Manage Jenkins → Configure System`  
- **Slack & Email Notifications** → Configure workspace and SMTP credentials

</details>

---

## ⚡ Example Pipeline Parameters
| Parameter Name              | Type     | Description                                           | Default |
|------------------------------|---------|-------------------------------------------------------|---------|
| `SKIP_STABILITY`            | Boolean | Skip the Code Stability stage                         | false   |
| `SKIP_QUALITY`              | Boolean | Skip the Code Quality Analysis stage                  | false   |
| `SKIP_COVERAGE`             | Boolean | Skip the Code Coverage Analysis stage                 | false   |
| `APPROVAL_REQUIRED`         | Boolean | Require approval before publishing artifacts         | true    |

### Check against build the required parameters:
<img width="911" height="603" alt="image" src="https://github.com/user-attachments/assets/3646af4c-12f6-45b0-8ca5-7433881da180" />

##  Jenkinsfile Breakdown

###  Checkout Stage

Fetches the Java project from GitHub.

```

stage('Checkout') {
    steps {
        git branch: 'master', url: 'https://github.com/opstree/spring3hibernate.git'
    }
}

```

###  Parallel Checks

Runs the following three scans in parallel to save time:

####  Code Stability

Runs unit tests using Maven and publishes JUnit results.

####  Code Quality Analysis

Executes Checkstyle, PMD, and FindBugs scans.
Publishes HTML reports for all.

####  Code Coverage

Generates JaCoCo coverage report and HTML view.


###  Packaging

Builds and archives the final WAR artifact.

     stage('Package') {
                steps {
                    echo ":package: Packaging application..."
                    sh '''
                        export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
                        export PATH=$JAVA_HOME/bin:$PATH
                        mvn clean package -DskipTests
                    '''
                }

###  Approval to Publish

Manual input or auto-approval based on user parameter.

```
     stage('Approval to Publish') {
                steps {
                    script {
                        if(!params.AUTO_APPROVE_PUBLISH) {
                            timeout(time: 5, unit: 'MINUTES') {
                                input message: ":white_check_mark: Approve artifact publishing?", ok: "Approve"
                            }
                        } else {
                            echo ":heavy_check_mark: Auto-approval enabled, proceeding to publish..."
                        }
                    }
                }
            }
```

### All the Coverages Report
<img width="1840" height="787" alt="image" src="https://github.com/user-attachments/assets/c4c458c8-50cc-4d5e-9e7f-91c564556bc5" />


**Summary**


Have successfully:


Built a declarative pipeline with conditional and parallel stages

Implemented approval-based publishing

Configured Slack and Email notifications

Managed artifacts and reports


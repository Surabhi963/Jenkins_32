#  Assignment-1

##  Overview
The **Assignment-1** This project helps practice Jenkins job creation, Git operations, parameterized builds, job chaining, and Slack & Email notifications.
This assignment has two major parts:


Part 1: Automate Git operations (create/list/merge/rebase/delete branches) in Jenkins.

Part 2: Create and publish a "Ninja" file across two chained Jenkins jobs.

---

## Project Name
**Assignment-1**

---

##  General Configuration

| Setting | Description |
|----------|-------------|
| **Project Description** | Assignment-1 |
| **Build Execution** | Enabled |
| **Concurrent Builds** | Disabled – ensures one build runs at a time |
| **GitHub Project** | Disabled (manual configuration used) |
| **Parameterized Build** | Disabled (static configuration) |
| **Throttle Builds** | Disabled (no concurrent restrictions) |
| **SCM Configuration** | Enabled (Git selected) |

---

##  Source Code Management (SCM)

###  Git Repository Configuration

| Configuration Option | Description |
|------------------------|-------------|
| **SCM Type** | Git |
| **Repository URL** | `https://github.com/Surabhi963/jenkins-lib.git` |
| **Credentials** | Surabhi963/****** |
| **Branch to Build** | Default (master or main) |

---

###  Explanation of Configuration

####  Repository URL

     https://github.com/Surabhi963/jenkins-lib.git


<img width="1342" height="572" alt="image" src="https://github.com/user-attachments/assets/69a7ccdf-0eee-485d-b692-b88fd87ff69c" />




##  Build Step Details

###  Script Type
**Execute Shell (Bash Script)**

###  Script Purpose
This script automates common Git operations within Jenkins — eliminating manual Git command execution.  
It performs the following sequential tasks:
1. Create a new branch  
2. List all branches  
3. Merge a feature branch into the main branch  
4. Rebase the feature branch  
5. Delete the feature branch  

---

##  Script Code


          set -e  # fail fast
          
          git fetch origin --prune
          git config user.name "jenkins-bot"
          git config user.email "jenkins-bot@example.com"
          
          case "$ACTION" in
            create)
              [ -z "$BRANCH" ] && { echo "BRANCH required"; exit 1; }
              git checkout $BASE_BRANCH
              git pull origin $BASE_BRANCH
              git checkout $BRANCH || git checkout -b $BRANCH
              git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/Surabhi963/jenkins-lib.git $BRANCH
              ;;
            list)
              git branch -a
              ;;
            merge)
              [ -z "$SOURCE_BRANCH" ] || [ -z "$TARGET_BRANCH" ] && { echo "Source/Target required"; exit 1; }
              git checkout $TARGET_BRANCH
              git pull origin $TARGET_BRANCH
              git merge origin/$SOURCE_BRANCH -m "Merge $SOURCE_BRANCH -> $TARGET_BRANCH"
              git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/Surabhi963/jenkins-lib.git $TARGET_BRANCH
              ;;
            rebase)
              [ -z "$SOURCE_BRANCH" ] || [ -z "$TARGET_BRANCH" ] && { echo "Source/Target required"; exit 1; }
              git checkout $SOURCE_BRANCH
              git pull origin $SOURCE_BRANCH
              git rebase origin/$TARGET_BRANCH
              git push --force-with-lease https://$GIT_USERNAME:$GIT_PASSWORD@github.com/Surabhi963/jenkins-lib.git $SOURCE_BRANCH
              ;;
            delete)
              [ -z "$BRANCH" ] && { echo "BRANCH required"; exit 1; }
              git branch -D $BRANCH || true
              git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/Surabhi963/jenkins-lib.git --delete $BRANCH
              ;;
            *)
              echo "Invalid ACTION"; exit 1;;
          esac





<img width="1277" height="835" alt="image" src="https://github.com/user-attachments/assets/a729d691-f24c-4a2a-9f8d-8e7e96398bd5" />

<img width="1677" height="832" alt="image" src="https://github.com/user-attachments/assets/8315250d-3053-47f4-bf82-df647d0b9480" />



### 1. **set -e  # fail fast**
Instructs the shell to exit immediately if any command returns a non-zero (failure) exit status.
Prevents continuing after an error (e.g., merge conflict, bad checkout), which avoids corrupting branches or running unwanted Git commands.

---

### 2. **git fetch origin --prune**
Fetches all latest branch info from the remote repository (origin) and removes any local references to branches that no longer exist remotely.

---

### 3. git config user.name and password
Sets Git’s user identity for commits made by Jenkins

---

### 4. **case "$ACTION" i**
Starts a case statement (similar to switch in other languages) 


---

### 5. **Create Branch"**
Checks if the $BRANCH variable is empty.
Switches to the base branch
Updates the local base branch with the latest commits from remote.
Tries: git checkout $BRANCH → switch to the branch if it exists.
If fails: Creates a new branch using -b $BRANCH.
git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/... $BRANCH
Pushes the branch to remote GitHub using HTTPS authentication with Jenkins credentials.

---

### 6. **List Branches**
Lists all branches (-a shows local and remote branches).

---

### 7. **Merge Branches**
Checks that both source and target branch names are provided
Switch to the branch you want to merge into (the destination)
Make sure the target branch is up to date before merging
Merges the source branch from remote into the target branch.
Pushes the merged target branch back to GitHub.

---

### 8. **Rebase**
Verifies both SOURCE_BRANCH and TARGET_BRANCH are set
Switches to the branch you want to rebase.
Updates the source branch with any latest changes before rebasing
Reapplies commits from source on top of the latest target branch history.
Pushes the rebased branch back to remote.

---

### 9. **Delete Branch**
Ensures a branch name is provided before deletion.
Deletes the branch locally (-D = force delete).
Deletes the branch from the remote repository on GitHub.

---

### 10. **Default Case (Invalid Input)**
Prints an error and exits with failure code 1.

---

##  Summary of Environment Variables Used

| Variables | Purpose |
|----------|----------|
| `$ACTION` | Defines which Git operation to perform (create, list, etc.) |
| `$BRANCH` | Branch name (for create/delete) |
| `$BASE_BRANCH` | The base branch to create from (e.g., main) |
| `$SOURCE_BRANCH` | Branch being merged/rebased. |
| `$TARGET_BRANCH` | Branch to merge/rebase into |
| `$GIT_USERNAME / $GIT_PASSWORD` | Jenkins credentials used for pushing |

##  Post build Actions
<img width="1302" height="480" alt="image" src="https://github.com/user-attachments/assets/efe35e02-22b7-46af-af17-1e43573c53f0" />

---

##  Part 2a : Create Ninja.txt

     #!/bin/bash
     # Create a file
     FILE_NAME="ninja_file.txt"
     
     # Add content
     echo "${NINJA_NAME} from DevOps Ninja" > $FILE_NAME
     
     # Optional: print content to console
     cat $FILE_NAME

<img width="1236" height="463" alt="image" src="https://github.com/user-attachments/assets/fad75fb4-e3fe-49d0-a002-afa51f10ec4d" />

<img width="1338" height="485" alt="image" src="https://github.com/user-attachments/assets/46b07fd6-0cc5-4700-9ef1-39668dc0a31b" />

##  Part 2b : Publish Ninja.txt

     #!/bin/bash
     # Copy the artifact to web directory
     sudo cp /var/lib/jenkins/workspace/Assign1_Part2_create-ninja-file/ninja_file.txt /var/www/html/ninja_file.txt
     
     
     # Show content
     cat /var/www/html/ninja_file.txt
     
<img width="1261" height="370" alt="image" src="https://github.com/user-attachments/assets/609dab98-c1e7-43f9-98bc-3138052a0cec" />

<img width="1285" height="427" alt="image" src="https://github.com/user-attachments/assets/edaed00e-86c2-4fd2-9fce-c2033be5cef7" />


##  In Jenkins Context

Jenkins passes $ACTION, $BRANCH, etc., as parameters in the pipeline job.
Script can be placed in:
     Execute shell build step, or
     A reusable shared library file like git-ops.sh in your jenkins-lib repo.
     
When this job runs:
1. Jenkins starts a build and runs this script inside its workspace.
2. A file named **`ninja.txt`** is created in that workspace.
3. Jenkins shows both the creation message and the file’s content in the **Console Output**.
4. The file is then **archived as a build artifact**, so it can be downloaded later.
5. If something goes wrong, Jenkins marks the build as **failed** because of `set -e`.

---

##  Post-Build Actions

After the build completes:
- The file `ninja.txt` is archived (saved in Jenkins).
- An **email notification** is sent to the specified recipient (e.g., `surabhi28dec@gmail.com`)  
  - Jenkins will notify the user whether the build succeeded or became unstable.

## Create Ninja.txt
<img width="1152" height="646" alt="image" src="https://github.com/user-attachments/assets/20c4e09c-b674-4bbc-a2ae-3a13e2715749" />

## Publish Ninja.txt
<img width="1290" height="462" alt="image" src="https://github.com/user-attachments/assets/d905da8c-ec41-420e-8040-757de6a745d2" />

---

###  Final Outcome
At the end of the build:
- A new file named **`ninja.txt`** is successfully created.
- The file contains:

<img width="1338" height="485" alt="image" src="https://github.com/user-attachments/assets/46b07fd6-0cc5-4700-9ef1-39668dc0a31b" />

<img width="1285" height="427" alt="image" src="https://github.com/user-attachments/assets/edaed00e-86c2-4fd2-9fce-c2033be5cef7" />





# CI/CD 

## What is CI/CD?

CI = Continuous Integration CD = Continuous Delivery / Continuous
Deployment

CI/CD automates building, testing, packaging, and deploying
applications.

## Traditional Workflow

``` text
Developer ->Build - > Test -> Deploy -> Production
```

## CI/CD Workflow

``` text
Developer -> GitHub -> Jenkins -> Build -> Test -> Package -> Deploy -> Production
```
# What is Continuous Integration (CI)?

Continuous Integration is the process of automatically integrating code changes into a shared repository.

Whenever a developer pushes code:

- Source code is downloaded
- Dependencies are installed
- Application is built
- Tests are executed
- Build status is generated

### Goal

Detect bugs early.

### Example

```
Developer A ----\
                 \
Developer B -----> GitHub
                 /
Developer C ----/

         │
         ▼
      Jenkins

         │
         ▼
 Automatic Build & Test
```

---

# Benefits of CI

- Early bug detection
- Frequent code integration
- Better code quality
- Automated builds
- Faster releases

---

# What is Continuous Delivery (CD)?

Continuous Delivery automatically prepares the application for deployment.

Deployment to production requires manual approval.

```
Code

↓

Build

↓

Test

↓

Deploy to Staging

↓

Manual Approval

↓

Production
```

---

# Benefits

- Reliable releases
- Reduced deployment risks
- Easy rollback
- Faster production releases

---

#  What is Continuous Deployment?

Continuous Deployment automatically deploys every successful build to production.

No manual approval is required.

# Benefits

- Fast releases
- Zero manual deployment
- Continuous software updates


## CI/CD Tools

-   Jenkins
-   GitHub Actions
-   GitLab CI/CD
-   Azure DevOps
-   CircleCI
-   TeamCity
-   Bamboo
-   Travis CI
-   AWS CodePipeline

# Popular CI/CD Tools

| Tool              | Open Source | Company |
|-------------------|-------------|---------|
| Jenkins           | Yes | Jenkins Community |
| GitHub Actions    | Yes | GitHub |
| GitLab CI/CD      | Yes | GitLab |
| Azure DevOps      | No | Microsoft |
| CircleCI          | No | CircleCI |
| TeamCity          | No | JetBrains |
| Bamboo            | No | Atlassian |
| Travis CI           | Yes | Travis CI |
| Bitbucket Pipelines | No | Atlassian |
| AWS CodePipeline | No | AWS |


=======================================================================================

## Jenkins

Jenkins is an open-source automation server used to automate CI/CD
pipelines.

### Features

-   Open source
-   Java based
-   Plugin ecosystem
-   Pipeline as Code
-   Distributed builds



# Jenkins Architecture

Jenkins follows a **Controller-Agent Architecture**.

```
                 +----------------------+
                 |      Developer       |
                 +----------+-----------+
                            |
                            |
                       Push Code
                            |
                            ▼
                 +----------------------+
                 |    GitHub / GitLab   |
                 +----------+-----------+
                            |
                     Webhook Trigger
                            |
                            ▼
              +-----------------------------+
              |     Jenkins Controller      |
              |-----------------------------|
              | Dashboard                   |
              | Scheduler                   |
              | Plugins                     |
              | Credentials                 |
              | Pipeline                    |
              +-------------+---------------+
                            |
          ------------------------------------------
          |                    |                   |
          ▼                    ▼                   ▼
 +----------------+   +----------------+   +----------------+
 | Jenkins Agent1 |   | Jenkins Agent2 |   | Jenkins Agent3 |
 | Linux          |   | Windows        |   | Docker/K8s     |
 +-------+--------+   +-------+--------+   +-------+--------+
         |                    |                    |
         ▼                    ▼                    ▼
      Build App           Run Tests         Deploy App
```

---

# Jenkins Controller Responsibilities

- Manage Jenkins
- Schedule jobs
- Install plugins
- Store configurations
- Manage credentials
- Assign work to agents

---

# Jenkins Agent Responsibilities

- Execute builds
- Run tests
- Deploy applications
- Execute pipelines

---

# Advantages of Agents

- Faster builds
- Parallel execution
- Reduced controller load
- Distributed builds

---

# 8. Jenkins Components

```
Jenkins

│

├── Dashboard

├── Jobs

├── Build Queue

├── Build History

├── Plugins

├── Credentials

├── Nodes (Agents)

├── Pipelines

└── Workspace
```

## Pipeline

A pipeline is an automated workflow defined in a Jenkinsfile.

Typical stages: 1. Checkout 2. Build 3. Test 4. Package 5. Docker Build
6. Push Image 7. Deploy 8. Notify

Example:

``` groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps { echo 'Building...' }
        }
        stage('Test') {
            steps { echo 'Testing...' }
        }
        stage('Deploy') {
            steps { echo 'Deploying...' }
        }
    }
}
```

## Summary

-   CI automates build and test.
-   CD automates delivery/deployment.
-   Jenkins is a popular CI/CD tool.
-   Controller-Agent architecture enables distributed builds.
-   Pipelines automate the software delivery lifecycle.


# Example : Go app deployment
```bash
go test ./... -v
sudo docker build -t naveen65/jenkins-demo:latest .
echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
sudo docker push naveen65/jenkins-demo:latest
sudo docker rm -f $(sudo docker inspect -f '{{.Id}}' go-app) || true
sudo docker run -d --name go-app -p 8081:8080 naveen65/jenkins-demo:latest
```

# Example : Pipeline for go-app


```groovy
pipeline {
    agent any

    environment {
        DOCKER_CREDS = credentials('dockerhub_cred')
    }

    stages {

        stage('Git Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Naveenkumar511/Devops-go-app-project.git'
            }
        }

        stage('Testing') {
            steps {
                sh 'go test ./... -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t naveen65/jenkins-demo:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                sh '''
                echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push naveen65/jenkins-demo:latest'
            }
        }

        stage('Remove Existing Container') {
            steps {
                sh '''
                docker rm -f go-app || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker run -d --name go-app -p 8081:8080 naveen65/jenkins-demo:latest
                '''
            }
        }
    }
}
```
# Example : Pipeline for go-app

```groovy
pipeline {
    agent any

   
    stages {

        stage('Git Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Naveenkumar511/Devops-go-app-project.git'
            }
        }

        stage('Testing') {
            steps {
                sh 'go test ./... -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t naveen65/jenkins-demo:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub_creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                         )
            ])      {
            sh '''
            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
            '''
                }
             }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push naveen65/jenkins-demo:latest'
            }
        }

        stage('Remove Existing Container') {
            steps {
                sh '''
                docker rm -f go-app || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker run -d --name go-app -p 8081:8080 naveen65/jenkins-demo:latest
                '''
            }
        }
    }
}
```
# Configuring agent in jenkins

A Jenkins **Controller** manages Jenkins and schedules jobs.

A Jenkins **Agent (Worker Node)** executes the build, test, and deployment jobs assigned by the Controller.

Using agents improves:

- Scalability
- Parallel Builds
- Performance
- Security

---

# 2. Prerequisites

You need two Ubuntu EC2 instances.

| Server | Purpose |
|---------|----------|
| Jenkins Controller | Jenkins Installed |
| Jenkins Agent | Executes Jobs |

Example

```text
Controller EC2

IP : 54.xx.xx.xx

Agent EC2

IP : 13.xx.xx.xx
```

---

# 3. Architecture

```text
                 Developer
                      │
                      ▼
                  GitHub
                      │
                      ▼
            Jenkins Controller
          (Schedules the Build)
                      │
                SSH Port 22
                      │
                      ▼
             Jenkins Agent
         ├── Build
         ├── Test
         ├── Docker Build
         └── Deploy
```

# Steps to connect agent 
# Login to agent server
1. Install open jdk in agent server
```bash
sudo apt install openjdk-21-jdk -y
```
2. Create jenkins user and give sudo permission
```bash
sudo useradd -m -s /bin/bash jenkins
sudo passwd jenkins 
visudo 
    jenkins   ALL:ALL NOPASSWD: ALL
```

# Log in to jenkins controller server
1. Login as jenkins user
```bash
sudo su - jenkins
```
2. Create passwordless authendication
```bash
ssh-keygen # enter for all 

ssh-copy-id -i /var/lib/jenkins/.ssh/id_ed25519.pub jenkins@<private_ip_of_agent>
```

## OR

```bash
cat /var/lib/jenkins/.ssh/id_ed25519.pub
# copy the public key 
# login to agent server as jenkins user
mkdir .ssh
vim .ssh/authorized_keys
    #paste the copied public key & save it

```

Finally test the passwordless authendication working or not form jenkins server
```bash
# login to jenkins server
ssh jenkins@<private_ip_of_agent>
# it will login without asking password
```

# Login jenkins web consone
1. Go to "manage jenkins" > "security" > "credentials" > "add" > "Add SSH Username with private key"

fill id , username  & private key > "enter directly"
# login to jenkins server as jenkins user & copy paste the private key
```bash
cat /var/lib/jenkins/.ssh/id_ed25519
```

Go to "manage jenkins" > "system" > "nodes" > add nodes


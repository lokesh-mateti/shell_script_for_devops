Continuous Integration and Continuous Deployment (CI/CD) pipelines are fundamental in DevOps for automating the build, test, and deployment processes. Here's an example of a CI/CD pipeline using declarative Jenkins pipeline syntax along with shell scripts for each stage.

### Declarative Pipeline Example

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                git 'https://github.com/your/repository.git'
            }
        }
        
        stage('Build') {
            steps {
                // Execute shell script for build
                sh '''
                    #!/bin/bash
                    echo "Building application..."
                    mvn clean install
                '''
            }
        }
        
        stage('Unit Test') {
            steps {
                // Execute shell script for unit tests
                sh '''
                    #!/bin/bash
                    echo "Running unit tests..."
                    mvn test
                '''
            }
        }
        
        stage('Integration Test') {
            steps {
                // Execute shell script for integration tests
                sh '''
                    #!/bin/bash
                    echo "Running integration tests..."
                    mvn verify
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                // Execute shell script for deployment
                sh '''
                    #!/bin/bash
                    echo "Deploying application..."
                    ansible-playbook deploy.yml -i inventory/hosts
                '''
            }
        }
    }
    
    post {
        success {
            echo 'CI/CD pipeline succeeded!'
            // Additional actions on success
        }
        failure {
            echo 'CI/CD pipeline failed!'
            // Additional actions on failure
        }
    }
}
```

### Explanation

- **Agent:** `agent any` specifies that Jenkins can execute this pipeline on any available agent.
- **Stages:** Each stage represents a phase of the CI/CD process.
  - **Checkout:** Checks out the code from the Git repository.
  - **Build:** Builds the application using Maven.
  - **Unit Test:** Runs unit tests using Maven.
  - **Integration Test:** Runs integration tests using Maven.
  - **Deploy:** Deploys the application using Ansible.
- **Steps:** Within each stage, `sh ''' ... '''` is used to define and execute shell scripts. Replace `mvn` and `ansible-playbook` commands with your specific build and deployment commands.
- **Post-actions:** Defines actions to be taken after the pipeline completes. Here, `success` and `failure` blocks handle actions based on pipeline outcome.

### Notes

- **Environment Setup:** Ensure Jenkins has necessary plugins (like Git and Maven) installed and configured.
- **Credentials:** Manage credentials securely in Jenkins for accessing Git repositories or deployment tools.
- **Error Handling:** Implement error handling, logging, and notifications as per your organization's requirements.

This example provides a basic structure for a CI/CD pipeline using Jenkins declarative syntax and shell scripts. Customize and expand it according to your project's specific needs, such as adding additional stages (like security scans, performance tests) or integrating with other tools (like Docker, Kubernetes).

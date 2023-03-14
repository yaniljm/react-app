pipeline {
    agent any
    tools {
        nodejs "node"
        jfrogCli "jfrog-cli"
    }
    environment {
        NPM_REGISTRY = "https://acndevops.jfrog.io/artifactory/api/npm/devops-local/"
        JFROG_CREDS_ID = "jfrog-credentials"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yaniljm/react-app.git'
            }
        }
        
        stage('Build') {
            steps {
                bat 'npm install'
                bat 'npm run build'
            }
        }
        
        stage('Publish') {
            steps {
                withCredentials('final-devops') {
                    bat "npm config set registry https://acndevops.jfrog.io/artifactory/api/npm/devops-local/"
                    bat "npm publish --registry=https://acndevops.jfrog.io/artifactory/api/npm/devops-local/"
                }
                rtNpmPublish(
                    tool: 'jfrog-cli',
                    deployerId: 'jfrog-instance',
                    path: 'npm-repo/@mycompany',
                    npmArgs: '--registry=${env.NPM_REGISTRY}'
                )
            }
        }
    }
}

pipeline {
    agent any
    tools {
        nodejs "node"
        jfrogCli "jfrog-cli"
    }
    environment {
        NPM_REGISTRY = "https://acndevops.jfrog.io/artifactory/api/npm/devops-local/"
        JFROG_INSTANCE = "https://acndevops.jfrog.io/"
        JFROG_CREDS_ID = 'final-devops'
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
                    bat "npm config set registry ${env.NPM_REGISTRY}"
                    bat "npm login --scope=@mycompany --registry=${env.NPM_REGISTRY} <<EOF\n${USERNAME}\n${PASSWORD}\n${USER_EMAIL}\nEOF"
                    bat "npm publish --registry=${env.NPM_REGISTRY}"
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

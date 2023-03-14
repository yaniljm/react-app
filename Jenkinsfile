pipeline {
    agent any
    tools {
        nodejs "node"
    }
    environment {
        NPM_REGISTRY = "https://acndevops.jfrog.io/artifactory/api/npm/devops-local/"
        JFROG_INSTANCE = "https://acndevops.jfrog.io/"
        JFROG_CREDS_ID = 'JFrog'
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
                withCredentials([usernamePassword(credentialsId: env.JFROG_CREDS_ID, usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    bat "npm config set registry ${env.NPM_REGISTRY}"
                    bat "npm login --scope=@mycompany --registry=${env.NPM_REGISTRY} <<EOF\n${USERNAME}\n${PASSWORD}\n${USER_EMAIL}\nEOF"
                    bat "npm publish --registry=${env.NPM_REGISTRY}"
                }
                rtNpmPublish(
                    tool: 'jfrog-cli',
                    serverId: 'jfrog-instance',
                    registry: env.NPM_REGISTRY,
                    repo: 'npm-repo',
                    npmArgs: '--scope=@mycompany'
                )
            }
        }
    }
}

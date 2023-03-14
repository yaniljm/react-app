pipeline {
    agent any
    environment {
        ARTIFACTORY_URL = 'https://acndevops.jfrog.io/'
        JFROG_USER = credentials('devops')
        JFROG_API_KEY = credentials('cmVmdGtuOjAxOjE3MTAyOTc3NjY6TFR0Snp2YXloaW9uOU8zTlh2Z0tGbGRJV2pk')
        NPM_REGISTRY = "https://my-npm-registry.com"
    }
    
    stages {
        stage('SCM') {
            steps {
                git 'https://github.com/yaniljm/react-app.git'
                checkout scm
            }
        }
        
        stage('Install dependencies') {
            steps {
                bat 'npm install'
                bat 'npm test'
            }
        }
        
        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }
        stage('Package and Publish') {
            steps {
                // Set NPM registry to use JFrog
                bat "npm config set registry ${env.NPM_REGISTRY}"
                bat "npm config set '//${env.NPM_REGISTRY}/:_authToken' \$(echo -n ${env.JFROG_USER}:${env.JFROG_API_KEY} | base64)"
                
                // Publish package to JFrog
                bat 'npm publish'
            }
        }
    }
}

pipeline {
  agent any
  
  environment {
    CI = true
    ARTIFACTORY_URL = "https://acndevops.jfrog.io"
    ARTIFACTORY_CRED = credentials('artifactory-token)
  }
  
  stages {
    stage("SCM") {
      steps {
        git 'https://github.com/yaniljm/react-app.git'
        sh 'npm install'
      }
    }
    stage("Build") {
      steps {
        bat 'npm run build'
        bat 'npm install -g serve'
      }
    } 
    stage("Publish") {
      steps {
        bat 'npm pack'
        bat 'jfrog rt npm-publish --npm-auth .npmrc --build-name my-package --build-number 1.0.0 .tgz'
      }
    }
  }
}

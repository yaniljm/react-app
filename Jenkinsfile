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
        bat 'jfrog rt upload --url https://acndevops.jfrog.io/articatory/ --access-token ${ARTIFACTORY_CRED} target/demo-0.0.1-SNAPSHOT.jar devops-local/'
      }
    }
  }
}

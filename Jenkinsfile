pipeline {
  agent any
  
  environment {
    ARTIFACTORY_URL = "https://acndevops.jfrog.io"
    ARTIFACTORY_USER = "devops"
    ARTIFACTORY_PASSWORD = "ACNDevops.fy23"
    NPMRC_CONTENTS = "registry=$https://acndevops.jfrog.io/artifactory/api/npm/devops-local\n_auth=$devops:$ACNDevops.fy23\nemail=devops@gmail.com"
  }
  
  Stages{
    stage("SCM") {
        git 'https://github.com/yaniljm/react-app.git'
        bat 'npm install'
    }
    stage("Build") {
        bat 'npm run build'
        bat 'npm install -g serve'
    } 
    stage("Publish") {
      bat 'npm pack'
      bat 'jfrog rt npm-publish --npm-auth .npmrc --build-name my-package --build-number 1.0.0 .tgz'
    }
  }
}

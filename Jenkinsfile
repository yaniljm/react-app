node {
  stage('SCM') {
    git 'https://github.com/yaniljm/react-app.git'
  }
  stage('Publish') {
            steps {
                script {
                    def server = Artifactory.server 'artifactory-server'
                    def npm = Artifactory.newNpmBuild()
                    
                    npm.tool = 'nodejs'
                    npm.args = 'run build'
                    npm.publish = true
                    npm.deployer releaseRepo:'npm-release-local', snapshotRepo:'npm-snapshot-local', server: server, registry: 'https://acndevops.jfrog.io/artifactory/api/npm/devops-local/'
                    
                    npm.run()
                }
            }
  }
}

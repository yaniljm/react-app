node {
    def server = Artifactory.server('artifactory-server')
    def rtNpm = Artifactory.newNpmBuild()
    def buildInfo = Artifactory.newBuildInfo()
    
    stages {
        stage('SCM') {
            steps {
                git 'https://github.com/yaniljm/react-app.git'
            }
        }
        
        stage('Install dependencies') {
            steps {
                bat 'npm install'
            }
        }
        
        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }
        
        stage('Publish') {
            steps {
                script {
                    def buildInfo = rtBuildInfo()
                    
                    rtNpmSetRegistry(registry: "${https://acndevops.jfrog.io}/${devops-local}")
                    //rtNpmAuth(authParams: [username: "${devops", password: "$cmVmdGtuOjAxOjE3MTAyOTc3NjY6TFR0Snp2YXloaW9uOU8zTlh2Z0tGbGRJV2pk", email: 'devops@gmail.com'])
                    bat "npm publish ${devops} --registry=${https://acndevops.jfrog.io}/${devops-local}"
                    
                    buildInfo.appendBuildInfo(env.JOB_NAME, env.BUILD_NUMBER, env.GIT_COMMIT, 'npm')
                    rtPublishBuildInfo serverId: 'Artifactory', buildInfo: buildInfo
                }
            }
        }
    }
}

node {
    def server = Artifactory.server('artifactory-server')
    def rtNpm = Artifactory.newNpmBuild()
    def buildInfo = Artifactory.newBuildInfo()
    stage 'Build'
        git url: 'https://github.com/yaniljm/react-app.git'

    stage 'Artifactory configuration'
        rtNpm.tool = 'JFrog' // Tool name from Jenkins configuration
        rtNpm.deployer repo:'npm-dev-local',  server: server
        rtNpm.resolver repo:'npm-dev', server: server

        stage('Config Build Info') {
            buildInfo.env.capture = true
            buildInfo.env.filter.addInclude("*")
        }
        stage('Install dependencies') {
            steps {
                bat 'npm install'
            }

}

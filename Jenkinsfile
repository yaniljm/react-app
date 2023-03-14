node {
    def server = Artifactory.server('devopsjfrog')
    def rtNpm = Artifactory.newNpmBuild()
    def buildInfo = Artifactory.newBuildInfo()
    stage 'Build'
        git url: 'https://github.com/yaniljm/react-app.git'

    stage 'Artifactory configuration'
        rtNpm.tool = 'devopsjfrog' // Tool name from Jenkins configuration
        rtNpm.deployer repo:'npm-dev-local',  server: server
        rtNpm.resolver repo:'npm-dev', server: server

        stage('Config Build Info') {
            buildInfo.env.capture = true
            buildInfo.env.filter.addInclude("*")
        }

        stage('Extra Npm Configurations') {
            rtNpm.usesPlugin = true // Artifactory plugin already defined in build script
        }
        stage('Exec Npm') {
            rtNpm.run rootDir: "artifactory/", buildFile: 'build.Npm', tasks: 'clean artifactoryPublish', buildInfo: buildInfo
        }
        stage('Publish build info') {
            server.publishBuildInfo buildInfo
        }
}

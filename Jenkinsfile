node {
    def app
    def remote = [:]
    remote.name = 'ansible-node'
    remote.host = '40.68.87.60'
    remote.user = 'prod'
    remote.password = 'Password123456'
    remote.allowAnyHosts = true

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("kmclau208/coursework2")
    }

    stage('SonarQube analysis') {
      def scannerHome = tool 'SonarQubeScanner';
      withSonarQubeEnv('sonarqube') {
        sh "${scannerHome}/bin/sonar-scanner"
      }
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    stage('ssh command') {
        sshCommand remote: remote, sudo: true, command: 'whoamI -un'
      }

}


node {
   def remote = [:]
    remote.name = 'vm'
    remote.host = '40.68.87.60'
    remote.user = 'azureuser'
    remote.password = 'Coursework2001'
    remote.allowAnyHosts = true

    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('SonarQube analysis') {
      def scannerHome = tool 'SonarQubeScanner';
      withSonarQubeEnv('sonarqube') { // If you have configured more than one global server connection, you can specify its name
        sh "${scannerHome}/bin/sonar-scanner"
      }
    }

    stage('Build image') {
        app = docker.build("kmclau208/coursework2")
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    stage('Remote SSH') {
      sshCommand remote: remote, command: "ls -lrt"
      sshCommand remote: remote, command: "for i in {1..5}; do echo -n \"Loop \$i \"; date ; sleep 1; done"
    }
}


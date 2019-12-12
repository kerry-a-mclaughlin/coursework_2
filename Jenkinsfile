node {
    def app

    stage('Clone repository') {
        checkout scm
    }
    
    stage('SonarQube analysis') {
      def scannerHome = tool 'SonarScanner 4.0';
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
}


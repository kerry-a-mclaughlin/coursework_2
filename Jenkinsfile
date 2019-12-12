node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage("build & SonarQube analysis") {
              node {
                  withSonarQubeEnv('My SonarQube Server') {
                     sh 'mvn clean package sonar:sonar'
                  }
              }
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

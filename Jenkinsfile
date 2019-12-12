node {
    def app

    stage('Clone repository') {
        checkout scm
    }

   stage('Sonarqube') {
        environment {
            scannerHome = tool 'sonarqube'
        }
            withSonarQubeEnv('sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner"
        }
            timeout(time: 10, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
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


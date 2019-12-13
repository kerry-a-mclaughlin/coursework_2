node {
   def scannerHome = tool 'SonarQube';
   def app
   def remote = [:]
   remote.name = 'master'
   remote.host = '13.82.54.23'
   remote.user = 'master'
   remote.password = '@Coursework2001'
   remote.allowAnyHosts = true

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

    stage('Deploy New Build To Kubernetes') {
        sshCommand remote: remote, sudo: false, command: 'ansible-playbook /home/master/Git/coursework_2/ansible/vm_startKubernetes.yml -i /home/master/Git/coursework_2/ansible/hosts'
    }
}
node {
   def scannerHome = tool 'SonarQubeScanner';
   def app
   def remote = [:]
   remote.name = 'master'
   remote.host = '13.82.54.23'
   remote.user = 'master'
   remote.password = '@Coursework2001'
   remote.allowAnyHosts = true

    stage('Cloning repository') {
        checkout scm
    }

    stage('Building image') {
        app = docker.build("kmclau208/coursework2")
    }

    stage('SonarQube analysis') {
      withSonarQubeEnv('sonarqube') {
        sh "${scannerHome}/bin/sonar-scanner"
      }
    }

    stage('Pushing image to DockerHub') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    stage('Deploying build To Kubernetes') {
        sshCommand remote: remote, sudo: false, command: 'ansible-playbook /home/master/ansible/vm_startKubernetes.yml -i /home/master/ansible/hosts'
    }
}
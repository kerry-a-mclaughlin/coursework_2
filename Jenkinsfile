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

    stage('Deploy New Build To Kubernetes') {
        sshCommand remote: remote, sudo: false, command: 'ansible-playbook /home/master/Git/coursework_2/ansible/vm_startKubernetes.yml -i /home/master/Git/coursework_2/ansible/hosts'
    }

}

=======
    stage('Deploying build To Kubernetes') {
        sshCommand remote: remote, sudo: false, command: 'ansible-playbook /home/master/ansible/vm_kubernetes.yml -i /home/master/ansible/hosts'
    }
}
>>>>>>> d3aba925e307c1a091c8a426884f804e73b3286e

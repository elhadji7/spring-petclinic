pipeline {
    agent any
    
    stages {
        stage('Récupération du code') {
            steps {
                git branch: 'main',
         credentialsId: 'GithubCredentials',
          url: 'https://github.com/elhadji7/spring-petclinic.git'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw package'
            }
        }
         stage('Deploy') {
             steps {
                 script {
                     def remote = [
                         name: 'distantServ',
                         host: '192.168.196.153',
                         user: 'user',
                         port: 22,
                         allowAnyHosts: true
                     ]
                     withCredentials([sshUserPrivateKey(credentialsId: 'SSHkeysPEM', keyFileVariable: 'keyFile', passphraseVariable: 'pass', usernameVariable: 'user')]) {
                         sshPublisher(publishers: [sshPublisherDesc(configName: remote.name, transfers: [
                             sshTransfer(cleanRemote: false, execCommand: "mkdir -p /home/user/build/"),
                             sshTransfer(cleanRemote: false, execCommand: "scp -i $keyFile target/*.jar ${remote.user}@${remote.host}:/home/user/build/")
                         ])])
                     }
                  }  
              }
          }
     }
}

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
                     withCredentials([sshUserPrivateKey(credentialsId: 'SSHkeysPEM', keyFileVariable: 'keyFile', passphraseVariable: 'pass', usernameVariable: 'user')]) {
                         def remote = [
                             name: 'distantServ',
                             host: '192.168.196.153',
                             user: user,
                             identityFile: keyFile,
                             allowAnyHosts: true
                        ]
                        sshPublisher(publishers: [sshPublisherDesc(configName: remote.name, transfers: [
                            sshTransfer(execCommand: 'mkdir -p /home/user/build/'),
                            sshTransfer(execCommand: "scp -i ${keyFile} target/*.jar ${remote.user}@${remote.host}:/home/user/build/")
                        ])])
                    }
              }
           }
        }
     }
}

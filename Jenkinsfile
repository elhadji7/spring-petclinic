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
                          sshCommand remote: remote, command: 'mkdir /home/user/built'
                          sshCommand remote: remote, command: 'for i in {1..5}; do echo -n "loop$i"; hostname -I; sleep 1; done'
                          writeFile file: 'abc.sh', text: 'ls -lrt'
                          sshScript remote: remote, script: 'sh abc.sh'
                          sshPut remote: remote, from: 'target/*.jar', into: '/home/user/built/'
                      }
                   }
               }
            }
    }
}

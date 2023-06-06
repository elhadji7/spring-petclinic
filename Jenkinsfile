pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/elhadji7/spring-petclinic.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package -f pom.xml'
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    def remote = [:]
                    remote.name = 'distantServ'
                    remote.host = '192.168.196.153'
                    remote.user = 'user'
                    remote.port = 22
                    remote.allowAnyHosts = true
                    
                        withCredentials([sshUserPrivateKey(credentialsId: 'SSHkeysPEM', keyFileVariable: 'keyFile', passphraseVariable: 'pass', usernameVariable: 'user')]) {
                        sshPublisher(publishers: [sshPublisherDesc(configName: remote.name, transfers: [
                            sshTransfer(execCommand: "mkdir -p /home/user/build/",
                                        execTimeout: 120000),
                            sshTransfer(execCommand: "scp -i $keyFile */target/*.jar ${remote.user}@${remote.host}:/home/user/build/",
                                        execTimeout: 120000)
                        ])])
                    }
                }
            }
        }
    }
}

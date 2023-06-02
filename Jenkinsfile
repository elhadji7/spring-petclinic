pipeline {
    agent any
    
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package -f pom.xml'
            }
        }
        
        stage('Convert JAR to WAR') {
            steps {
                script {
                    sh '''
                        # Create a directory to hold the WAR structure
                        mkdir war
                        cd war

                        # Copy the JAR file into the WEB-INF/lib directory
                        mkdir -p WEB-INF/lib
                        cp ../target/*.jar WEB-INF/lib

                        # Create the web.xml file if needed
                        touch WEB-INF/web.xml

                        # Create the necessary directories and files
                        mkdir WEB-INF/classes
                        # Copy additional resources as needed

                        # Package the WAR file
                        jar -cf ../target/my-app.war .
                    '''
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'scp target/*.war <username>@<server>:<destination>'
            }
        }
    }
}

pipeline {
    agent none
    stages{
        stage('Java Build') {
            agent {
                docker {
                    image 'maven:3-alpine' 
                    args '-v /root/.m2:/root/.m2' 
                }
            }
            steps {
                    sh 'mvn -f java-project/pom.xml -B -DskipTests clean package'
                }
            }
        
        stage('Building Account Service'){
            agent any
            steps {
                sh '''
                    cd java-project/account-service/
                '''
                dir('java-project/account-service'){
                    def customImage = docker.build("account_service")
                    customImage.push()
                }
            }
        }
    }
}

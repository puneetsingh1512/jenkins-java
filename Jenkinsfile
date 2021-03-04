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
                    stash includes: 'java-project/account-service/target/account-service-1.0-spring-boot.jar', name: 'account_jar'   
                }
            }
        
        stage('Building Account Service'){
            agent any
            steps {
                dir ('java-project/account-service/target'){
                    unstash 'account_jar'
                }
                sh 'ls --list'
                sh 'docker build -f java-project/account-service/Dockerfile .'
            }
        }
    }
}

pipeline {
    agent any
    stages{
        agent {
            docker {
                image 'maven:3-alpine' 
                args '-v /root/.m2:/root/.m2' 
            }
        }
        stages {
            stage('Java Build') { 
                steps {
                    sh 'mvn -f java-project/pom.xml -B -DskipTests clean package' 
                }
            }
        }
        stage('Building Account Service'){
            steps {
                sh 'docker.build -f java-project/account-service/Dockerfile'
            }
        }
    }
}
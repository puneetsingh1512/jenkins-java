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
                    stash includes: 'java-project/account-service/target/**/*', name: 'account_jar'   
                }
            }
        
        stage('Building Account Service'){
            agent any
            steps {
                dir('java-project/account-service/target')
                {
                    unstash 'account_jar'
                }
                sh 'docker build -f java-project/account-service/Dockerfile .'
            }
        }
    }
}

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
        
        stage('Account Service Build and Push'){
            agent any
            environment {
                Dockerhub_cred = credentials('dockerhub')
            }
            steps {
                sh '''
                    cd java-project/account-service/
                    docker build -t puneetss/account_service .
                    docker login -u $Dockerhub_cred_USR -p $Dockerhub_cred_PSW
                    docker push puneetss/account_service:latest
                ''' 
            }
        }
    }
}

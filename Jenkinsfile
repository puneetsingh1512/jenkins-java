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
        stage('Customer Service Build and Push'){
            agent any
            environment {
                Dockerhub_cred = credentials('dockerhub')
            }
            steps {
                sh '''
                    cd java-project/customer-service/
                    docker build -t puneetss/customer_service .
                    docker login -u $Dockerhub_cred_USR -p $Dockerhub_cred_PSW
                    docker push puneetss/customer_service:latest
                ''' 
            }
        }
        stage('SonarQube analysis') {
            agent {
                docker {
                    image 'maven:3-alpine' 
                    args '-v /root/.m2:/root/.m2' 
                }
            }
            steps {
                withSonarQubeEnv(credentialsId: '9d63a8c5e7fd530181f1b2f985ee5ed6f55c0f91', installationName:'Sonarqube') {
                    // Optionally use a Maven environment you've configured already
                    withMaven(maven:'Maven 3.5') {
                        sh 'mvn -f java-project/pom.xml clean package sonar:sonar'
                    }
                }
            }
        }
    }
}

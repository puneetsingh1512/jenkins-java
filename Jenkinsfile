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
                    docker build -t puneetss/account_service .
                ''' 
                withDockerRegistry([ credentialsId: "bdcfb6fe-9dce-4e42-82d5-55ff687f4f18", url: "" ]) {
                  sh 'docker push puneetss/account_service:latest'
                }
            }
        }
    }
}

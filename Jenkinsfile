pipeline {
    agent  any
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn java-project/pom.xml -B -DskipTests clean package' 
            }
        }
    }
}

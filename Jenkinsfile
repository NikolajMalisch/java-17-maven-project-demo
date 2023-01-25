pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh "/opt/apache-maven-3.8.6/bin/mvn package"
            }
        }
        stage('Test') {
            steps {
                sh "/opt/apache-maven-3.8.6/bin/mvn test"
            }
        }
        stage('Release') {
            steps{
             withCredentials([usernamePassword(credentialsID: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])  
              sh '''
              docker build -t nikolajm/java17:${BUILD_NUMBER} ."
              docker login -u $USERNAME -p $PASSWORD
              docker push  nikolajm/java17:${BUILD_NUMBER}
              '''
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }
}

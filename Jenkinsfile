pipeline {
    agent any
    stages {
        stage('Sonar check quality') {
            agent {
            docker.withTool('docker'){
                docker.withRegistry('repo','credentials') {
                    image 'maven:3.8.1-adoptopenjdk-11'
                }
            }
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') 
                    {
						sh 'mvn clean package sonar:sonar'
					}
                }
            }
        }
    }
}
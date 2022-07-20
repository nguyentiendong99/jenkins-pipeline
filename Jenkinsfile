pipeline {
    agent any
    stages {
        stage('Sonar check quality') {
            agent {
                docker {
                    image 'maven:3.8.1-adoptopenjdk-11'
                }
            }
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') 
                    {
						sh 'mvn verify sonar:sonar -Dsonar.login=admin -Dsonar.password=PhucYem1966#'
					}
                }
            }
        }
    }
}
pipeline {
    agent any
    stages {
        stage('Sonar check quality') {
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') 
                    {
						sh 'mvn verify sonar:sonar -Dsonar.login=admin -Dsonar.password=PhucYem1966#'
					}
                    // timeout(time: 1, unit: 'HOURS') {
                    //     def qg = waitForQualityGate() {
                    //         if(qg.status != 'OK') {
                    //             error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    //         }
                    //     }
                    // }
                }
            }
        }
    }
}
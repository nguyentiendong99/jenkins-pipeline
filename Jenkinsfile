pipeline {
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
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
        stage("docker build & docker push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                             sh '''
                                docker build -t 192.168.49.1:8085//springapp:${VERSION} .
                                docker login -u admin -p $docker_password 192.168.49.1:8085/
                                docker push  192.168.49.1:8085//springapp:${VERSION}
                                docker rmi 192.168.49.1:8085//springapp:${VERSION}
                            '''
                    }
                }
            }
        }
    }
}
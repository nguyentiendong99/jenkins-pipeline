pipeline {
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages {
        // stage('Sonar check quality') {
        //     agent {
        //         docker {
        //             image 'maven:3.8.1-adoptopenjdk-11'
        //         }
        //     }
        //     steps {
        //         script {
        //             withSonarQubeEnv(credentialsId: 'sonar-token') 
        //             {
		// 				sh 'mvn verify sonar:sonar -Dsonar.login=admin -Dsonar.password=PhucYem1966#'
		// 			}
        //         }
        //     }
        // }
        stage("docker build & docker push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                             sh '''
                                docker build -t 192.168.49.1:8085/springapp:${VERSION} .
                                docker login -u admin -p $docker_password 192.168.49.1:8085/
                                docker push  192.168.49.1:8085/springapp:${VERSION}
                                docker rmi 192.168.49.1:8085/springapp:${VERSION}
                            '''
                    }
                }
            }
        }
        stage('Identifying misconfigs using datree in helm charts') {
            steps {
                script {
                    dir('kubernetes/') {
                        withEnv(['DATREE_TOKEN=4c6b1670-8aef-4ed8-8a33-f6596e3ca90b']) {
                            sh 'helm datree test myapp/'
                        }
                    }
                }
            }
        }
    }
    post {
		always {
			mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "dong19069999@gmail.com";  
		 }
	   }
}
pipeline {
    agent { label 'jenkins-agent' }
    tools {
        jdk 'java17'
        maven 'Maven3'
    }
	 environment {
	    APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "devsecops12345"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}" 
	  }
	
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/alain-stan-devops/register-app'
                }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }

       stage("SonarQube Analysis"){
           steps {
	             script {
		              withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') { 
                      sh "mvn sonar:sonar"
            	              }
                 }
	         }	
         }  

	            stage("Build & Push Docker Image") {
            steps {
                   withCredentials([dockerRegistry(credentialsId: 'dockerhub')]) {
       sh 'docker login -u $USERNAME -p $PASSWORD'
       sh 'docker build .'
       sh 'docker push my-image'
   }
   

                    }
                }
            }

       }

    }
}

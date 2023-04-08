pipeline {

    agent any

    environment {
		DOCKERHUB_CREDENTIALS=credentials('josiokoko')
	}
	
    stages {
    
        stage("SCM Checkout"){
            steps{
                git branch: 'main', url: 'https://github.com/josiokoko/serverapp.git'
            }
        }

        
        stage("Docker Build"){
            steps {
                sh "docker build -t josiokoko/raycoy-app ."
            }
        }


         stage("Authenticate to DockerHub") {
            steps {
                script {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    echo 'Login complete...'
                }
            }
        }

        
        stage("Push Image to Registry") {
            steps {
                script {
                    sh "docker tag josiokoko/raycoy-app docker.io/josiokoko/raycoy-webapp:$BUILD_ID"
                    sh "docker push docker.io/josiokoko/raycoy-webapp:$BUILD_ID"
                }
            }
        }

    
        stage("Remove Docker Image") {
            steps{
                sh 'docker rmi -f $(docker images -q)'	
                sh 'docker logout'  
            }
        }

           
    }
}
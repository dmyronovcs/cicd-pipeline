pipeline {
    agent any
   
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('DockerCred')
        DOCKER_IMAGE_NAME = 'dmyronovcs/nodedev'
        DOCKER_IMAGE_TAG = 'v1.0'
    }
   
    stages {
        stage('DEV Checkout') {
            steps {
                // Checkout the specific branch from your Git repository
                script {
                    checkout([$class: 'GitSCM', branches: [[name: "*/dev"]], extensions: [], userRemoteConfigs: [[url: 'https://github.com/dmyronovcs/cicd-pipeline.git']]])
                }
            }
        }
        
        stage('DEV Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
		                sh 'pwd'
                    sh 'docker build -t dmyronovcs/nodedev:v1.0 .'
                    
                }
            }
        }
        
        stage('DEV Test') {
            steps {
                script {
                    // Test the Docker image
		            sh '''
		                docker run --rm -d -p 3001:3000 --name nd1 dmyronovcs/nodedev:v1.0 && sleep 20 && curl localhost:3001
		                docker container stop nd1
		            '''
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DockerCred', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                        sh "docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD"
                        sh "docker push $DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG"
                    }
                }
            }
        }
        
        // Add more stages for your deployment or other tasks
    }
}

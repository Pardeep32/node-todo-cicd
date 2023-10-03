pipeline {
    agent { label 'dev-agent' }
    stages {
        stage('code') {
            steps {
                echo "Cloning the code"
                git url: 'https://github.com/Pardeep32/node-todo-cicd.git', branch: 'master'
            }
        }
        stage('build') {
            steps {
                echo "Building the Docker image"
                sh 'docker build . -t pardeepkaur/node-cicd-app:latest'
            }
        }
        stage('login and push image') {
            steps {
                echo "Login to Docker Hub and push the image"
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh 'docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PASS'
                    sh 'docker push pardeepkaur/node-cicd-app:latest'
                }
            }
        }
        stage('deploy') {
            steps {
                echo "Deploying the container"
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}

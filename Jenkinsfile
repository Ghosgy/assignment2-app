pipeline {
    agent any

    environment {
        DOCKERHUB_REPO = "dre1lly/runcalc-pro"
        IMAGE_TAG = "v1.0.${BUILD_NUMBER}"
    }

    stages {
        stage('Get Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_REPO:$IMAGE_TAG .'
            }
        }

        stage('Tag Images') {
            steps {
                sh '''
                    docker tag $DOCKERHUB_REPO:$IMAGE_TAG $DOCKERHUB_REPO:latest
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $DOCKERHUB_REPO:$IMAGE_TAG
                        docker push $DOCKERHUB_REPO:latest
                    '''
                }
            }
        }
    }
}

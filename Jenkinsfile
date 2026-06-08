pipeline {
    agent any

    environment {
        IMAGE_NAME = "adityakulkarni548/sample-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/Aditya422-ai/finacplus-assesment.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $IMAGE_NAME:$IMAGE_TAG
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([
                    file(credentialsId: 'kubeconfig',
                    variable: 'KUBECONFIG')
                ]) {

                    sh '''
                    sed -i "s|IMAGE_PLACEHOLDER|$IMAGE_NAME:$IMAGE_TAG|g" deployment.yaml

                    kubectl apply -f deployment.yaml
                    '''
                }
            }
        }
    }

    post {

        success {
            echo 'Deployment Successful'
        }

        failure {
            echo 'Pipeline Failed'
        }

        always {
            cleanWs()
        }
    }
}

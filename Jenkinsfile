pipeline {
    agent any

    environment {
        IMAGE_NAME = "adityakul548/sample-app"
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

        stage('Debug Kubeconfig') {
    steps {
        withCredentials([
            string(credentialsId: 'kubeconfig', variable: 'KUBE_CONFIG_CONTENT')
        ]) {
            sh '''
            echo "$KUBE_CONFIG_CONTENT" > kubeconfig.yaml

            echo "===== FIRST 20 LINES ====="
            head -20 kubeconfig.yaml
            '''
        }
    }
}   
       stage('Deploy') {
    steps {
        sh '''
        sed -i "s|IMAGE_PLACEHOLDER|adityakul548/sample-app:${BUILD_NUMBER}|g" deployment.yaml

        kubectl apply -f deployment.yaml

        kubectl get pods
        kubectl get svc
        '''
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

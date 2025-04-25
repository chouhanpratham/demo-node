pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'prathamchouhan/mac-node-app'
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/chouhanpratham/demo-node.git' // ✅ Replace with your actual repo if different
            }
        }

        stage('Build Docker Image') {
            steps {
                // Change to the node-app directory
                    dir('node-app') {
                        // Build Docker image from within the node-app folder
                        sh 'docker build -t $DOCKER_IMAGE .'
                    }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: "${DOCKER_CREDENTIALS_ID}",
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh '''
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                            docker push $DOCKER_IMAGE
                            docker logout
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Successfully pushed $DOCKER_IMAGE to Docker Hub!"
        }
        failure {
            echo "❌ Build or push failed. Check logs for details."
        }
    }
}

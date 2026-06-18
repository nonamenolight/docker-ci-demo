pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE = "docker-ci-demo"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh """
                docker build -t ${IMAGE}:${TAG} .
                docker tag ${IMAGE}:${TAG} ${REGISTRY}/${IMAGE}:${TAG}
                """
            }
        }

        stage('Push to Local Registry') {
            steps {
                sh """
                docker push ${REGISTRY}/${IMAGE}:${TAG}
                """
            }
        }

        stage('Run Test Container') {
            steps {
                sh """
                docker run --rm ${REGISTRY}/${IMAGE}:${TAG}
                """
            }
        }
    }

    post {
        success {
            echo "CI/CD SUCCESS: ${IMAGE}:${TAG}"
        }
        failure {
            echo "CI/CD FAILED"
        }
    }
}

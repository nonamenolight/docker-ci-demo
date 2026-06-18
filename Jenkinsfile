pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE = "docker-ci-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm

                script {
                    env.GIT_SHORT_COMMIT = sh(
                        script: "git rev-parse --short HEAD",
                        returnStdout: true
                    ).trim()

                    env.VERSION = "${BUILD_NUMBER}-${GIT_SHORT_COMMIT}"
                }
            }
        }

        stage('Build Image') {
            steps {
                sh """
                echo "Building version: ${VERSION}"

                docker build -t ${IMAGE}:${VERSION} .

                docker tag ${IMAGE}:${VERSION} ${REGISTRY}/${IMAGE}:${VERSION}
                """
            }
        }

        stage('Push Image') {
            steps {
                sh """
                docker push ${REGISTRY}/${IMAGE}:${VERSION}
                """
            }
        }

        stage('Verify Run') {
            steps {
                sh """
                docker run --rm ${REGISTRY}/${IMAGE}:${VERSION}
                """
            }
        }
    }

    post {
        success {
            echo "SUCCESS: ${REGISTRY}/${IMAGE}:${VERSION}"
        }
    }
}

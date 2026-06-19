pipeline {
    agent any

    environment {
        REGISTRY = "127.0.0.1:5000"
        IMAGE_NAME = "docker-ci-demo"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Detect Dockerfile') {
            steps {
                script {
                    DOCKERFILE_PATH = sh(
                        script: "find $WORKSPACE -name Dockerfile | head -n 1",
                        returnStdout: true
                    ).trim()

                    echo "Dockerfile found at: ${DOCKERFILE_PATH}"
                }
            }
        }

        stage('Build Image with Kaniko') {
            steps {
                sh """
                docker run --rm \
                  -v $WORKSPACE:/workspace:rw \
                  gcr.io/kaniko-project/executor:latest \
                  --context=dir:///workspace \
                  --dockerfile=${DOCKERFILE_PATH} \
                  --destination=${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG} \
                  --destination=${REGISTRY}/${IMAGE_NAME}:latest \
                  --cache=true \
                  --cache-repo=${REGISTRY}/cache \
                  --cleanup \
                  --verbosity=info
                """
            }
        }

        stage('Verify Image') {
            steps {
                sh """
                docker images | grep ${IMAGE_NAME} || true
                """
            }
        }
    }

    post {
        success {
            echo "CI SUCCESS: Image pushed successfully"
        }
        failure {
            echo "CI FAILED"
        }
        always {
            cleanWs()
        }
    }
}

pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t docker-ci-demo:latest .'
            }
        }

        stage('Run') {
            steps {
                sh 'docker run --rm docker-ci-demo:latest'
            }
        }
    }

    post {
        success {
            echo "CI SUCCESS"
        }
        failure {
            echo "CI FAILED"
        }
    }
}

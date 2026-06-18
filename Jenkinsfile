
 peline {
    agent any

    environment {
        IMAGE = "docker-ci-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t $IMAGE:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run --rm $IMAGE:latest'
            }
        }
    }

    post {
        success {
            echo 'CI SUCCESS'
        }
        failure {
            echo 'CI FAILED'
        }
    }
}CI from Jenkins + Docker")

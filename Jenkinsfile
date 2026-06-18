
 peline {
    agent any

    environment {
        IMAGE = "docker-ci-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                git credentialsId: 'github-ssh-key',
                    url: 'git@github.com:nonamenolight/docker-ci-demo.git',
                    branch: 'main'
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
}

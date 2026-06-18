pipeline {
    agent any

    environment {
        REGISTRY = "127.0.0.1:5000"
        IMAGE = "docker-ci-demo"
        TAG = "kaniko-${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

       stage('Build with Kaniko') {
	    steps {
		sh '''
		ls -R $WORKSPACE

		docker run --rm \
		  --network=host \
		  -v $WORKSPACE:$WORKSPACE \
		  gcr.io/kaniko-project/executor:latest \
		  --context=dir://$WORKSPACE \
		  --dockerfile=Dockerfile \
		  --destination=127.0.0.1:5000/docker-ci-demo:${BUILD_NUMBER} \
		  --insecure \
		  --skip-tls-verify
		'''
	    }
	}
        

        stage('Verify Image') {
            steps {
                sh "curl -s http://${REGISTRY}/v2/_catalog || true"
            }
        }
    }

    post {
        success {
            echo "Kaniko CI SUCCESS: ${REGISTRY}/${IMAGE}:${TAG}"
        }
        failure {
            echo "Kaniko CI FAILED"
        }
    }
}

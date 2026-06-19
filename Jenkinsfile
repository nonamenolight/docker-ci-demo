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
   	stage("Find out workspace"){
 	    steps {
		sh '''
    	    	    echo "WORKSPACE=$WORKSPACE"
	    	    ls -la $WORKSPACE
	    	    find $WORKSPACE -name Dockerfile
		'''
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

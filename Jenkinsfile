pipeline {
    agent {
        node {
            label "Agent-node-1"
        }
    }
    environment {
        DOCKER_IMAGE = "jagan2810/jenkins-nodejs-app"
        DOCKER_TAG = "${BUILD_NUMBER}"
        DOCKER_CRED_ID = "Docker-credentials"
    }
    stages {
        stage("Build Info") {
            steps {
                sh """
                echo "Build started"
                echo "Build Number: ${BUILD_NUMBER}"
                """
            }
        }
        
        stage("Install Dependencies") {
            steps {
                sh "npm install"
            }
        }
        
        stage("Run Test Case") {
            steps {
                sh "npm test"
            }
        }
        
        stage("Docker Build") {
            steps {
                sh """
                echo "Docker image is building...."
                docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest
                """
            }
        }
        
        stage("Docker Image Push") {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CRED_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo "Docker image is pushing...."
                    echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                    docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                    docker push ${DOCKER_IMAGE}:latest
                    """
                }
            }
        }
    }
    
    post {
        always {
            sh "docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest || true"
            sh "docker logout || true"
        }
    }
}
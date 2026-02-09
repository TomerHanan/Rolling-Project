pipeline {
    agent {
        label 'agent2'
    }

    environment {
        // Correct way to handle credentials to avoid Groovy interpolation warnings
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id') 
        IMAGE_NAME = "your-dockerhub-username/rolling-project"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Initialize & Check') {
            steps {
                // Verify the files actually exist before starting
                sh '''
                    echo "Checking workspace content:"
                    ls -F
                    if [ ! -f Dockerfile ]; then
                        echo "ERROR: Dockerfile not found in root!"
                        exit 1
                    fi
                '''
            }
        }

        stage('Parallel Checks') {
            parallel {
                stage('Linting') {
                    steps {
                        echo "Running Flake8 linting..."
                        // Note: Ensure the 'python/' directory exists in your repo root
                        sh 'docker run --rm -v $(pwd):/app -w /app python:3.11-slim sh -c "pip install flake8 --quiet && flake8 . --max-line-length=120 --ignore=E501,W503"'
                        
                        echo "Running Hadolint for Dockerfile..."
                        sh 'docker run --rm -v $(pwd):/app -w /app hadolint/hadolint < Dockerfile || true'
                    }
                }

                stage('Security Scan') {
                    steps {
                        echo "Running Bandit for Python security..."
                        sh 'docker run --rm -v $(pwd):/app -w /app python:3.11-slim sh -c "pip install bandit --quiet && bandit -r . -f txt"'
                        
                        echo "Running Trivy security scan..."
                        sh 'docker run --rm -v $(pwd):/app aquasec/trivy:latest fs --severity HIGH,CRITICAL --exit-code 0 /app'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh '''
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    docker push ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
        success {
            echo "Pipeline succeeded! Image pushed: ${IMAGE_NAME}:${IMAGE_TAG}"
        }
        failure {
            echo "Pipeline failed! Check the logs above for specific tool errors."
        }
    }
}
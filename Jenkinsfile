/*pipeline {
    agent any 

    stages {
        stage('Build') {
            steps {
                script {
                    // Build and push Docker image
                    bat 'docker build -t w9-dd-app:latest .'
                    bat 'docker tag w9-dd-app:latest wilsonbolledula/w9-dh-app:latest'
                    bat 'docker push wilsonbolledula/w9-dh-app:latest'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Delete and start Minikube cluster
                    bat 'minikube delete'
                    bat 'minikube start'
                    
                    // Enable the dashboard addon
                    bat 'minikube addons enable dashboard'
                    
                    // Apply Kubernetes resources
                    bat 'kubectl apply -f my-kube1-deployment.yaml'
                    bat 'kubectl apply -f my-kube1-service.yaml'
                    
                    // Expose the Kubernetes Dashboard service
                    bat 'minikube dashboard'
                    
                    echo 'Deploying application...'
                }
            }
        }
    }
}
*/
pipeline {
    agent any

    environment {
        IMAGE_NAME = "wilsonbolledula/w9-dh-app:latest"
    }

    stages {
        stage('Build and Push Docker Image') {
            steps {
                script {
                    bat "docker build -t ${IMAGE_NAME} ."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        bat "docker login -u %USER% -p %PASS%"
                        bat "docker push ${IMAGE_NAME}"
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running Selenium tests...'
                    // Add your test commands here
                    // bat 'python tests/seleniumDockerTest.py'
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    bat 'minikube status || minikube start'
                    bat 'minikube addons enable dashboard'
                    bat 'kubectl apply -f my-kube1-deployment.yaml'
                    bat 'kubectl apply -f my-kube1-service.yaml'
                    bat 'kubectl rollout status deployment/my-kube1-deployment'
                    bat 'minikube dashboard --url'
                    echo '✅ Deployment Successful!'
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        // Define Docker Hub credentials ID
        DOCKERHUB_CREDENTIALS = 'dockerhub'
        // Define Docker image details
        DOCKER_IMAGE = 'vangapallybhanu6659/myhtmlapp1'
        // Define Docker container name
        CONTAINER_NAME = 'mycontainer2'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                git branch: 'main', url: 'https://github.com/Bhanu-Vangapally/cicd.git'
            }
        }

        stage('Build') {
            steps {
                // Build Docker image
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push') {
            steps {
                // Push Docker image to Docker Hub
                script {
                    docker.withRegistry('', 'dockerhub') {
                        docker.image('vangapallybhanu6659/myhtmlapp1').push()
                    }
                }
            }
        }

        stage('Pull') {
            steps {
                // Pull Docker image from Docker Hub
                script {
                   docker.withRegistry('', 'dockerhub') {
                        docker.image('vangapallybhanu6659/myhtmlapp1').pull()
                }
            }
          }
        }
        stage('Run') {
            steps {
                // Run Docker container
               script {
                    docker.withRegistry('', 'dockerhub') {
                        docker.image('vangapallybhanu6659/myhtmlapp1').run('-d -p 8093:80 --name mycontainer2')
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline successfully completed!'
        }
        failure {
            echo 'Pipeline failed :('
        }
        always {
            script {
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
                sh "docker rmi ${DOCKER_IMAGE} || true"
            }
        }
    }
}

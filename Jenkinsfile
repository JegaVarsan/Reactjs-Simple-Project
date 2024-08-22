pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-react-app:${env.BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/JegaVarsan/Reactjs-Simple-Project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using Windows command
                    bat "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove the previous container if it exists
                    def containerId = bat(script: "docker ps -q --filter name=my-react-app-container", returnStdout: true).trim()
                    if (containerId) {
                        bat "docker stop ${containerId}"
                        bat "docker rm ${containerId}"
                    }
                    
                    // Run the new container
                    bat "docker run -d --name my-react-app-container -p 3000:3000 ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace
            cleanWs()
        }
    }
}

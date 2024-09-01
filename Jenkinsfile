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
                    // Use sudo to build Docker image
                    sh "sudo docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove the previous container if it exists using sudo
                    def containerId = sh(script: "sudo docker ps -q --filter name=my-react-app-container", returnStdout: true).trim()
                    if (containerId) {
                        sh "sudo docker stop ${containerId}"
                        sh "sudo docker rm ${containerId}"
                    }

                    // Run the new container with sudo
                    sh "sudo docker run -d --name my-react-app-container -p 3000:3000 ${DOCKER_IMAGE}"
                }
            }
        }
    }
}

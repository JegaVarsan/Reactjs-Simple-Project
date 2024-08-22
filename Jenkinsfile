pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-react-app:${env.BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/JegaVarsan/Reactjs-Simple-Project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove the previous container if it exists
                    def containerId = sh(script: "docker ps -q --filter name=my-react-app-container", returnStdout: true).trim()
                    if (containerId) {
                        sh "docker stop ${containerId}"
                        sh "docker rm ${containerId}"
                    }
                    
                    // Run the new container
                    sh "docker run -d --name my-react-app-container -p 3000:3000 ${DOCKER_IMAGE}"
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

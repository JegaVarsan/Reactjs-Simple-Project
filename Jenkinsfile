pipeline {
    // Using Docker as the agent with Docker-in-Docker enabled
    agent {
        docker {
            image 'docker:24.0-dind'  // Use a recent Docker-in-Docker image
            args '-v /var/run/docker.sock:/var/run/docker.sock --privileged'
        }
    }

    environment {
        DOCKER_IMAGE = "my-react-app:${env.BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Cloning the React application repository from GitHub
                git branch: 'master', url: 'https://github.com/JegaVarsan/Reactjs-Simple-Project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Building a Docker image using the Dockerfile in the cloned repository
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
                        // Stopping the existing container
                        sh "docker stop ${containerId}"
                        // Removing the stopped container
                        sh "docker rm ${containerId}"
                    }

                    // Running the newly built Docker image in a container
                    sh "docker run -d --name my-react-app-container -p 3000:3000 ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        always {
            // Cleaning up the workspace to maintain a clean environment
            cleanWs()
        }
    }
}

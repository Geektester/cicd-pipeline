pipeline {
    agent any

    environment {
        IMAGE_NAME = "geektester/geektesterimage"
    }

    stages {
    
     stage('Build Application') {
            steps {
                sh 'chmod +x scripts/build.sh'
                sh 'scripts/build.sh'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'chmod +x scripts/test.sh'
                sh 'scripts/test.sh'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    app = docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry(
                        'https://registry.hub.docker.com',
                        'docker_hub_creds_id'
                    ) {
                        app.push("latest")
                    }
                }
            }
        }
    }
}

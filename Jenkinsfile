pipeline {
    agent any

    parameters {
        string(defaultValue: '52.201.212.127', description: 'Host IP Address', name: 'HOST_IP')
        string(defaultValue: 'laravel-crud-boilerplate', description: 'Docker Repository Name', name: 'DOCKER_REPO_NAME')
        string(defaultValue: 'php-rest', description: 'Image Name', name: 'IMAGE_NAME')
        string(defaultValue: 'lr_app', description: 'Container Name', name: 'CONTAINER_NAME')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: 'main']],
                    userRemoteConfigs: [[url: 'https://github.com/pavankumar0077/laravel-crud-boilerplate.git']]
                ])  
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker-compose build"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'Docker-cred', url: 'https://index.docker.io/v1/', toolName: 'docker']) {
                        sh "docker-compose push ${DOCKER_REPO_NAME}/${IMAGE_NAME}"
                    }
                }
            }
        }

        stage('Run Application') {
            steps {
                script {
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            script {
                sh "docker-compose down"
            }
        }
    }
}

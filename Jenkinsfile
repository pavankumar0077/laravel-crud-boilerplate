pipeline {
    agent any

    parameters {
        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
        string(defaultValue: '52.201.212.127', description: 'Host IP Address', name: 'HOST_IP')
        string(defaultValue: 'pavan0077', description: 'Docker Repository Name', name: 'Docker Repository Name')
        string(defaultValue: 'php-rest', description: 'Image Name', name: 'Image Name')
        string(defaultValue: 'php-rest', description: 'Container Name', name: 'Container Name')
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




#pipeline {
    agent any

    environment {
        DEV_DOCKER_REPO = 'ppandian05/dev-repo'
        PROD_DOCKER_REPO = 'ppandian05/prod-repo'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    def app = docker.build("${env.DEV_DOCKER_REPO}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push to Docker Hub') {
            when {
                branch 'dev'
            }
            steps {
                script {
                    docker.withRegistry('', "${env.DOCKER_CREDENTIALS_ID}") {
                        def app = docker.build("${env.DEV_DOCKER_REPO}:${env.BUILD_NUMBER}")
                        app.push()
                    }
                }
            }
        }

        stage('Push to Prod Docker Hub') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('', "${env.DOCKER_CREDENTIALS_ID}") {
                        def app = docker.build("${env.PROD_DOCKER_REPO}:${env.BUILD_NUMBER}")
                        app.push()
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

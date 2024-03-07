pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS_ID = 'DOCKERHUB_CREDENTIALS_ID'
        IMAGE_NAME = 'aaronwang0509/amster'
        MAJOR_VERSION = '7'
        MINOR_VERSION = '30'
    }
    stages {
        stage('Build and Push amster Image') {
            steps {
                script {
                    def imageName = "${env.IMAGE_NAME}"
                    def fullVersion = "${env.MAJOR_VERSION}.${env.MINOR_VERSION}.${env.BUILD_NUMBER}"
                    dir('amster') {
                        docker.build("${imageName}:${fullVersion}")
                        docker.withRegistry('https://index.docker.io/v1/', env.DOCKERHUB_CREDENTIALS_ID) {
                            docker.image("${imageName}:${fullVersion}").push()
                        }
                    }
                }
            }
        }
        stage('Build and Push amster-build Image') {
            steps {
                script {
                    def imageName = "${env.IMAGE_NAME}-build"
                    def fullVersion = "${env.MAJOR_VERSION}.${env.MINOR_VERSION}.${env.BUILD_NUMBER}"
                    dir('amster-build') {
                        docker.build("${imageName}:${fullVersion}")
                        docker.withRegistry('https://index.docker.io/v1/', env.DOCKERHUB_CREDENTIALS_ID) {
                            docker.image("${imageName}:${fullVersion}").push()
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Build and push completed.'
        }
    }
}
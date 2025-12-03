pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nagababu92/petclinic"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'test',
                    url: 'https://github.com/nagababuhyd/Jenkins.git'
            }
        }

        stage('Maven Build & Test') {
            steps {
                bat "mvn clean package -DskipTests=false"
            }
        }

        stage('Docker Build') {
            steps {
                bat "docker build -t %DOCKER_IMAGE%:%BUILD_NUMBER% ."
                bat "docker tag %DOCKER_IMAGE%:%BUILD_NUMBER% %DOCKER_IMAGE%:latest"
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(
                                       credentialsId: '426d9ce3-4878-4ad6-9920-a56744a63f32',
                                       usernameVariable: 'DOCKER_USER',
                                       passwordVariable: 'DOCKER_PASS'
                                 )]) {

                    bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                    bat "docker push %DOCKER_IMAGE%:%BUILD_NUMBER%"
                    bat "docker push %DOCKER_IMAGE%:latest"
                }
            }
        }
    }
}

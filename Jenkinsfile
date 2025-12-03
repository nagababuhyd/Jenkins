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

        stage('Test Maven') {
            steps {
                bat "where mvn"
                bat "mvn -version"
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
                                       credentialsId: '99c20544-5e31-4a6f-8dba-0aea2ab59516',
                                       usernameVariable: 'DOCKER_USER',
                                       passwordVariable: 'DOCKER_PASS'
                                 )]) {

                    bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                    bat "docker push %DOCKER_IMAGE%:%BUILD_NUMBER%"
                    bat "docker push %DOCKER_IMAGE%:latest"
                }
            }
        }

      stage('Run Container on Different Port') {
            steps {
                bat "docker run -d -p 9090:8080 %DOCKER_IMAGE%:%BUILD_NUMBER%"
            }
        }

    }
}

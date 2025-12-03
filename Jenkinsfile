pipeline {
    agent any

    environment {
        DOCKER_CREDS = credentials('99c20544-5e31-4a6f-8dba-0aea2ab59516')
        DOCKER_IMAGE = "nagababu92/petclinic"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/nagababuhyd/Jenkins.git'

            }
        }

        stage('Maven Build & Test') {
            steps {
                sh 'mvn clean package -DskipTests=false'
            }
        }

        stage('Docker Build') {
            steps {
                sh """
                docker build -t $DOCKER_IMAGE:${BUILD_NUMBER} .
                docker tag $DOCKER_IMAGE:${BUILD_NUMBER} $DOCKER_IMAGE:latest
                """
            }
        }

        stage('Docker Login & Push') {
            steps {
                sh """
                echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin
                docker push $DOCKER_IMAGE:${BUILD_NUMBER}
                docker push $DOCKER_IMAGE:latest
                """
            }
        }
    }
}

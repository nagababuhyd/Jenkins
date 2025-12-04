pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'tegit-webhook',
                    url: 'https://github.com/nagababuhyd/Jenkins.git'
            }
        }

        stage('print hello worlf') {
            steps {
              echo "Hello World from Jenkins!"
           }
        }
    }
}

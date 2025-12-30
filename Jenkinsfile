pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ftou7bny/jenkins-pipeline-demo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t docker.io/fethibny/demoapp:v1.4 .'
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push docker.io/fethibny/demoapp:v1.4'
                }
            }
        }
    }
}

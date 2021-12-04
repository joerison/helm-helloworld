pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    }

    stages {
        stage('Docker Build') {
            steps{
                sh 'docker build -t helloworld-nginx ./helloworld-nginx'
            }
        }

        stage('Docker Publish') {
            steps {
                withDockerRegistry([credentialsId: "dockerhub-credentials", url: ""]) {
                    sh 'docker build -t helloworld-nginx ./helloworld-nginx'
                    sh 'docker tag helloworld-nginx joerison/helloworld-nginx'
                    sh "docker push joerison/helloworld-nginx:latest"
                }
            }
        }

        stage('Kubernetes Deploy') {
            steps{
                sh 'echo "deploying on kubernetes with helm..."'
            }
        }

    }

}



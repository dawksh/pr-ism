pipeline {
    agent any

    tools {
        nodejs 'node'
    }

    environment {
        DOCKER_IMAGE = 'prism:latest'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                checkout scm
                sh 'npm install'
            }
        }


        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('K8s Deploy') {
            steps {
                sh 'kubectl apply -f k8s/ --validate=false --insecure-skip-tls-verify'
                sh 'kubectl get pods -n default --insecure-skip-tls-verify'
                sh 'kubectl get services -n default --insecure-skip-tls-verify'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
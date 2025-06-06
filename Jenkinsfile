pipeline {
    agent any

    tools {
        nodejs "node22"
    }

    stages {
        stage('Install dependencies') {
            steps {
                sh "npm install"
            }
        }

        stage('Test') {
            steps {
                sh "node --test"
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build . --tag ttl.sh/myapp:2h"
                sh "docker push ttl.sh/myapp:2h"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubernetes-token', serverUrl: 'https://k8s:6443']) {
                    sh "kubectl apply -f myapp.yaml"
                }
            }
        }
    }
}

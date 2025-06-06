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

        stage('Docker Deploy') {
            steps {
                sshagent(['pk-test']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no laborant@docker 'docker pull ttl.sh/myapp:2h && docker run -d --name myapp -p 4444:4444 ttl.sh/myapp:2h'
                    '''
                }   
            }
        }
    }
}

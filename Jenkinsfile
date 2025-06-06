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

        stage('Deploy to target VM') {
            steps {
                sshagent(['pk-test']) {
                    sh 'scp -o StrictHostKeyChecking=no index.js package.json laborant@target:~/myapp/'
                    sh '''
                    ssh -o StrictHostKeyChecking=no laborant@target '
                        cd ~/myapp &&
                        npm install &&
                        sudo systemctl daemon-reload &&
                        sudo systemctl restart myapp
                    '
                    '''
                }
            }
        }
    }
}

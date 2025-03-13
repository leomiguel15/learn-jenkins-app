pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci #use for CI servers
                    npm run build
                    ls -la
                '''
            }
        }
    
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm start
                    ls -la
                    find build/index.html
                    npm test
                '''
            }
        }
    }
}

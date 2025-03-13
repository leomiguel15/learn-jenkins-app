pipeline {
    agent any

    triggers {
        githubPush()
    }

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
                    clearWS()
                    echo 'success trigger'
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
                    find build/index.html
                    npm test
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.51.0-noble'
                    reuseNode true
                    args '-u root:root'
                }
            }
            steps { 
                sh '''
                    npm install serve
                    nohup node_modules/.bin/serve -s build &
                '''
                sh 'npx playwright test' // Start the Playwright tests
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}

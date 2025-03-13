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
                    image 'mcr.microsoft.com/playwright:v1.51.0-noble' //playwrights has Docker image link: https://playwright.dev/docs/docker
                    reuseNode true
                }
            }
            steps {
                sh '''
                    //add the commands that needs to run
                    npm install -g serve
                    serve -s build
                    npx playwright test //start the playwright tests
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}

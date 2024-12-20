pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'b26ffd85-b97f-4483-8de3-f94fdfc84c2b'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                test -f build/index.html
                npm test
                '''
            }
    }

        stage('Deploy') {
                agent {
                    docker {
                        image 'node:22-alpine'
                        reuseNode true
                    }
                }
                steps {
                    sh '''
                        npm install netlify-cli 
                        node_modules/.bin/netlify --version
                        echo "Deploying to Netlify...  Site ID: $NETLIFY_SITE_ID"
                        node_modules/.bin/netlify status
                        node_modules/.bin/netlify deploy --dir=build  --prod
                    '''
                }
        }


    }
}

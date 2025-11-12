pipeline {
    agent {
        label 'prod-kopyst-app'
    }
    
    environment {
        DEPLOY_PATH = '/var/www/html'
        GIT_BRANCH = 'dev-kopyst-client'
    }
    
    stages {
        stage('Pull Latest Code') {
            steps {
                echo '📥 Pulling latest code from GitHub...'
                dir("${DEPLOY_PATH}") {
                    sh '''
                        git config --global --add safe.directory ${DEPLOY_PATH}
                        git fetch origin
                        git checkout ${GIT_BRANCH}
                        git pull origin ${GIT_BRANCH}
                    '''
                }
            }
        }
        
        stage('Set Permissions') {
            steps {
                echo '🔐 Setting permissions...'
                sh 'sudo chown -R www-data:www-data ${DEPLOY_PATH}'
            }
        }
        
        stage('Deployment Complete') {
            steps {
                echo '✅ Code deployed to /var/www/html!'
            }
        }
    }
    
    post {
        success {
            echo '🎉 Deployment completed successfully!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}

pipeline {
    agent {
        label 'prod-kopyst-app'
    }
    
    environment {
        DEPLOY_PATH = '/var/www/html'
        GIT_BRANCH = 'main'
    }
    
    stages {
        stage('Pull Latest Code') {
            steps {
                echo "📥 Pulling latest code to ${DEPLOY_PATH}..."
                sh """
                    cd ${DEPLOY_PATH}
                    sudo chown -R ubuntu:ubuntu ${DEPLOY_PATH}
                    git config --global --add safe.directory ${DEPLOY_PATH}
                    
                    echo "Current: \$(git log -1 --oneline)"
                    
                    # Discard any local changes and force update
                    git fetch origin
                    git checkout ${GIT_BRANCH}
                    git reset --hard origin/${GIT_BRANCH}
                    git pull origin ${GIT_BRANCH}
                    
                    echo "✅ Updated to: \$(git log -1 --oneline)"
                """
            }
        }
        
        stage('Set Permissions') {
            steps {
                echo '🔐 Setting permissions...'
                sh """
                    sudo chown -R www-data:www-data ${DEPLOY_PATH}
                    sudo chmod -R 755 ${DEPLOY_PATH}
                """
            }
        }
    }
    
    post {
        success {
            echo '🎉 Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}

pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: "node18"  // Node version added in Jenkins
        PATH = "${NODEJS_HOME}/bin:${env.PATH}"
        SSH_KEY = credentials('ssh-vasaperfume-frontend') // Jenkins Credential ID (SSH)
        SERVER = "ubuntu@13.202.254.147" // Update EC2 server IP
        DEPLOY_PATH = "/var/www/frontend" // Where build should go
    }

    stages {
        stage('Pull Code') {
            steps {
                git branch: 'main', url: 'https://github.com/prasad3366/vasafrontendddddddd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to Server') {
            steps {
                sh """
                ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${SERVER} 'mkdir -p ${DEPLOY_PATH}'

                scp -o StrictHostKeyChecking=no -i ${SSH_KEY} -r build/* ${SERVER}:${DEPLOY_PATH}/

                # Restart Nginx if required
                ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${SERVER} 'sudo systemctl restart nginx'
                """
            }
        }
    }

    post {
        success {
            echo "🚀 Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}

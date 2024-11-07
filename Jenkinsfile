pipeline {
    agent any

    environment {
        // Define environment variables for the second server
        SERVER_USER = 'ec2-user'  // the username used to access the second server
        SERVER_IP = '35.175.136.82'
        DEPLOY_PATH = '/var/www/html'  // the path where Nginx serves the content
        SSH_KEY_PATH = '/home/ec2-user/.ssh/git'  // path to your private SSH key
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull the latest code from GitHub
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Add build commands if necessary (e.g., npm install for Node.js apps)
                echo 'Building the project...'
            }
        }

        stage('Deploy') {
            steps {
                // Copy files to the second server using SCP
                sh '''
                scp -i $SSH_KEY_PATH -r * $SERVER_USER@$SERVER_IP:$DEPLOY_PATH
                '''
            }
        }

        stage('Restart Nginx') {
            steps {
                // Restart or reload Nginx to serve the updated index.html
                sh '''
                ssh -i $SSH_KEY_PATH $SERVER_USER@$SERVER_IP 'sudo systemctl restart nginx'
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}


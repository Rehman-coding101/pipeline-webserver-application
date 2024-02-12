pipeline {
    agent any

    environment {
        // Define environment variables
        EC2_HOST = '15.157.152.107'
        EC2_USER = 'ubuntu'
        // Assuming you've added your SSH private key to Jenkins' credentials store
        SSH_KEY_ID = 'c6c25b07-e106-4816-9573-aa9d10052e30'
    }

    stages {
        stage('Checkout') {
            steps {
                // Check out from GitHub
                git url: 'https://github.com/Rehman-coding101/pipeline-webserver-application.git', branch: 'main'
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // SSH and SCP commands to transfer files and restart Apache
                    sshagent(credentials: [SSH_KEY_ID]) {
                        // Copy files to the Apache directory on EC2
                        sh "sudo scp -o StrictHostKeyChecking=no -r index.html ${EC2_USER}@${EC2_HOST}:/var/www/html/"
                        // Optional: Run any commands on EC2, like setting permissions or restarting Apache
                        sh "ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} 'sudo systemctl restart apache2'"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful.'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
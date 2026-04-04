pipeline {
    agent any

    environment {
        // 'remote-server-password' is the ID of the secret we created in Step 2
        remotePassword = credentials('remote-server-password')
        remoteUser = 'cicduser' // CHANGE THIS to your Ubuntu username, e.g., 'root' or 'ubuntu'
        remoteHost = '192.168.31.31'        // CHANGE THIS to your server's IP address
        remotePath = '/var/www/html/'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github-ssh-key', 
                    url: 'git@github.com:tomatoscheduledemo148/jenkindemo.git'
            }
        }
        
        stage('Deploy to Web Server') {
            steps {
                // Using sshpass to copy all files from the workspace to the remote web root
                // The '-o StrictHostKeyChecking=no' avoids a prompt for accepting the host key for the first time.
                sh """
                    sshpass -p ${remotePassword} scp -o StrictHostKeyChecking=no -r \
                    ${WORKSPACE}/* ${remoteUser}@${remoteHost}:${remotePath}
                """
            }
        }
    }
    
    post {
        always {
            // Optional: Clean up the workspace after the build to save disk space
            cleanWs()
        }
    }
}
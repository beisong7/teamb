pipeline {
    agent any
    environment {
        SSH_CRED = credentials('server-key')
        def CONNECT = 'ssh -o StrictHostKeyChecking=no ubuntu@3.99.174.54'
    }
    stages {
        
        stage('Build') {
            steps {
                echo 'building app'
                sh "pwd"
                sh "ls"
                sh "zip -r webapp.zip ."
                sh "ls"
            }
        }

        stage('Artifact') {
            steps {
                echo 'deploy to nexus server'
                
                sh "ls"
                sh "zip -r webapp.zip ."
                sh "curl -v -u username:password --upload-file path/to/your/file.zip \
    http://your-nexus-server:port/repository/your-repository-name/your-group-id/your-artifact-id/your-version/your-artifact-id-your-version.zip"
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying app'
                sshagent(['server-key']) {
                    sh 'curl -u username:password -O \
    http://your-nexus-server:port/repository/your-repository-name/your-group-id/your-artifact-id/your-version/your-artifact-id-your-version.zip'
                    sh '$CONNECT "sudo apt install zip -y"'
                    sh '$CONNECT "sudo rm -rf /var/www/html/"'
                    sh '$CONNECT "sudo mkdir /var/www/html/"'
                    sh '$CONNECT "sudo unzip webapp.zip -d /var/www/html/"'
                }
            }
        }

        stage('Clean-Up') {
            steps {
                echo 'Remove existing files'
                deleteDir()
            }
        }
    }
    post {
        failure {
            echo 'Build failed. Cleaning up workspace...'
            cleanWs()
        }
    }
    


}
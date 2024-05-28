pipeline {
    agent any
    environment {
        SSH_CRED = credentials('webkey')
        def CONNECT = 'ssh -o StrictHostKeyChecking=no ubuntu@ec2-35-183-1-154.ca-central-1.compute.amazonaws.com'
    }
    stages {
        
        stage('Build') {
            steps {
                echo 'packaging app'
                sh "pwd"
                sh "ls"
                sh "zip -r webapp.zip ."
                sh "ls"
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying app'
                sshagent(['webkey']) {
                    sh 'scp -o StrictHostKeyChecking=no -i $SSH_CRED webapp.zip ubuntu@ec2-35-183-1-154.ca-central-1.compute.amazonaws.com:/home/ubuntu'
                    sh '$CONNECT "sudo apt install zip -y"'
                    sh '$CONNECT "sudo rm -rf /var/www/html/"'
                    sh '$CONNECT "sudo mkdir /var/www/html/"'
                    // sh '$CONNECT "unzip /home/ubuntu/webapp.zip -d /home/ubuntu/app"'
                    sh '$CONNECT "sudo unzip webapp.zip -d /var/www/html/"'
                }
            }
        }


        stage('Clean-Up') {
            steps {
                echo 'Remove existing files'
            }
        }
    }
}

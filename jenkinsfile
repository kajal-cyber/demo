pipeline {
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_credentials')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_credentials')
        PRI_KEY = credentials('AWS_privatekey_1')
    }
    agent any
  stages {
    stage('Provision Infrastructure') {
        steps {
            // credentialsId loading private key and storing in var
            withCredentials([sshUserPrivateKey(
                credentialsId: 'AWS_privatekey_1',
                keyFileVariable: 'SSH_KEY')])
            {
                sh 'cp "$SSH_KEY" /var/tmp/newkey.pem'
            }
        }
    }

    stage('checkout') {
            steps {
                 script{
                        dir("terraform")
                        {
                            git branch: 'main', url: 'https://github.com/kajal-cyber/demo.git'
                        }
                    }
                }
            }
        stage('Terraform init') {
            steps {
                 sh 'pwd;cd terraform/ ; terraform init'
            }
        }
       stage('Terraform plan') {
            steps {
                sh 'pwd;cd terraform/ ; terraform plan'
            }
        }
     
        stage('Terraform apply'){
            steps {
                sh 'pwd;cd terraform/ ; terraform apply --auto-approve'
            }
        }
        
    }
}

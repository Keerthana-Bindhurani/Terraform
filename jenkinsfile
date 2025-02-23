pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    stages {
        stage('Checkout Code') {
            steps {
                deleteDir()  // Clean workspace before fetching new code
                git branch: 'main', url: 'https://github.com/Keerthana-Bindhurani/Terraform.git'
            }
        }

        stage('Initialize Terraform') {
            steps {
                sh '''
                terraform init -input=false
                terraform validate
                '''
            }
        }

        stage('Plan Terraform') {
            steps {
                sh 'terraform plan -out=tfplan -input=false'
            }
        }

        stage('Apply Terraform') {
            steps {
                input message: 'Proceed with Terraform Apply?', ok: 'Apply'
                sh 'terraform apply -auto-approve tfplan -input=false'
            }
        }
    }

    post {
        success {
            echo "Terraform applied successfully!"
        }
        failure {
            echo "Terraform deployment failed. Check logs."
        }
    }
}

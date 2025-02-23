pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        PATH = "/usr/bin:$PATH"
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    echo "Checking out code from GitHub..."
                    deleteDir()
                    git branch: 'main', url: 'https://github.com/Keerthana-Bindhurani/Terraform.git'
                }
            }
        }

        stage('Initialize Terraform') {
            steps {
                script {
                    echo "Initializing Terraform..."
                    sh 'terraform init -input=false'
                }
            }
        }

        stage('Plan Terraform') {
            steps {
                script {
                    echo "Generating Terraform execution plan..."
                    sh 'terraform plan -out=tfplan -input=false'
                    stash name: 'terraform-plan', includes: 'tfplan' // ✅ Stash plan file
                }
            }
        }

        stage('Apply Terraform') {
            steps {
                script {
                    unstash 'terraform-plan' // ✅ Retrieve plan file
                    sh 'ls -l tfplan || echo "❌ tfplan file missing!"' // Debugging step
                    echo "Applying Terraform changes..."
                    sh 'terraform apply -auto-approve tfplan'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Terraform applied successfully!"
        }
        failure {
            echo "❌ Terraform deployment failed. Check logs."
        }
    }
}

pipeline {
    agent any

    parameters {
        booleanParam(name: 'SKIP_CHECKOUT', defaultValue: false, description: 'Skip the Checkout stage')
        booleanParam(name: 'SKIP_PLAN', defaultValue: false, description: 'Skip the Terraform Plan stage')
        booleanParam(name: 'AUTO_APPROVE', defaultValue: false, description: 'Automatically apply Terraform changes without approval')
    }

    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        PATH = "/usr/bin:$PATH"  // Ensure Terraform is in the PATH
    }

    stages {
        stage('Checkout Code') {
            when {
                expression { return !params.SKIP_CHECKOUT } // Skip if SKIP_CHECKOUT is true
            }
            steps {
                script {
                    echo "Checking out code from GitHub..."
                    deleteDir() // Clean workspace before fetching new code
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
            when {
                expression { return !params.SKIP_PLAN } // Skip if SKIP_PLAN is true
            }
            steps {
                script {
                    echo "Generating Terraform execution plan..."
                    sh 'terraform plan -out=tfplan -input=false'
                }
            }
        }

        stage('Approval') {
            when {
                expression { return !params.AUTO_APPROVE } // Run approval only if AUTO_APPROVE is false
            }
            steps {
                script {
                    input message: 'Proceed with Terraform Apply?', ok: 'Apply'
                }
            }
        }

        stage('Apply Terraform') {
            steps {
                script {
                    echo "Applying Terraform changes..."
                    sh 'terraform apply -auto-approve tfplan -input=false'
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

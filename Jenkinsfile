pipeline {
  agent any
  
  environment {
    AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
    AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
  }

  stages {
    stage('Checkout Code') {
      steps {
        script {
          // Fetch the main branch and use credentials if the repo is private
          git branch: 'main', 
              url: 'https://github.com/Keerthana-Bindhurani/Terraform.git', 
              credentialsId: 'github-credentials-id'  // <-- Replace with your actual credentials ID
        }
      }
    }

    stage('Install Terraform') {
      steps {
        sh '''
          if ! command -v terraform &> /dev/null
          then
            echo "Terraform not found! Installing..."
            sudo apt-get update && sudo apt-get install -y terraform
          else
            echo "Terraform is already installed."
          fi
          terraform --version
        '''
      }
    }

    stage('Initialize Terraform') {
      steps {
        sh 'terraform init'
      }
    }

    stage('Validate Terraform') {
      steps {
        sh 'terraform validate'
      }
    }

    stage('Plan Terraform') {
      steps {
        sh 'terraform plan'
      }
    }

    stage('Apply Terraform') {
      steps {
        sh 'terraform apply -auto-approve'
      }
    }
  }

  post {
    always {
      echo "Terraform Pipeline Execution Completed."
    }
    success {
      echo "✅ Terraform applied successfully!"
    }
    failure {
      echo "❌ Terraform pipeline failed! Check logs."
    }
  }
}

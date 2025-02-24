pipeline {
  agent any
  
  environment {
    AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
    AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
  }

  stages {
    stage('Checkout Code') {
      steps {
        git 'https://github.com/Keerthana-Bindhurani/Terraform.git'
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
}

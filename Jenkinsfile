pipeline {
  agent any

  tools {
    terraform 'Terraform'
  }

  environment {
    AWS_ACCESS_KEY_ID     = credentials('aws-secret-key-id')
    AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
  }

  stages {
    stage('tfsec'){
      agent {
        docker {
            image 'tfsec/tfsec-ci:v0.57.1'
        }
      }
      steps {
        sh ''' tfsec --no-color '''
      }
    }
    stage('Init Provider') {
      steps {
        sh 'terraform init'
      }
    }
    stage('Plan Resources') {
      steps {
        sh 'terraform plan'
      }
    }
    stage('Apply Resources') {
      input {
        message "Do you want to proceed for production deployment?"
      }
      steps {
        sh 'terraform apply -auto-approve'
      }
    }
  }
}

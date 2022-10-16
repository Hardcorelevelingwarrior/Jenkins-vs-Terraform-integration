pipeline {
  agent any

  tools {
    terraform 'Terraform'
  }

  environment {
    AWS_ACCESS_KEY_ID     = credentials('aws-secret-key-id')
    AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
  }
  cache(caches: [[$class: 'ArbitraryFileCache', cacheValidityDecidingFile: '', excludes: '', includes: '**/*', path: '.terraform.lock.hcl']], defaultBranch: '/main', maxCacheSize: 100000) {
    // some block
}

  stages {
    stage('Init Provider') {
      agent { label 'linux'}
      steps {
        sh 'terraform init -upgrade'
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
        sh 'terraform apply -auto-approve -no-color'
      }
    }
  }
}

pipeline {
 agent {
  docker {
    image 'hashicorp/terraform:1.7.5'
    args '--user root -v /var/run/docker.sock:/var/run/docker.sock -v /tmp:/tmp'
  }
}

  environment {
    AWS_ACCESS_KEY_ID     = credentials('aws-access-key-id')
    AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/thegagankapoor/terraform-jenkins-pipeline.git'
      }
    }
    stage('Terraform Init') {
      steps {
        sh 'cd terraform-infra && terraform init'
      }
    }
    stage('Terraform Plan') {
      steps {
        sh 'cd terraform-infra && terraform plan'
      }
    }
    stage('Terraform Apply') {
     steps {
        input message: 'Approve to apply the Terraform plan?'
        sh 'cd terraform-infra && terraform apply -auto-approve'
      }
    }
  }
}

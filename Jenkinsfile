pipeline {
  agent any

  parameters {
    choice(name: 'action', choices: ['plan', 'apply', 'destroy'], description: 'Terraform action to perform')
    booleanParam(name: 'autoApprove', defaultValue: false, description: 'Auto approve without manual input?')
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
        sh 'terraform -chdir=terraform-infra init'
      }
    }

    stage('Terraform Action') {
      steps {
        script {
          if (params.action == 'plan') {
            sh 'terraform -chdir=terraform-infra plan'
          } else if (params.action == 'apply') {
            if (!params.autoApprove) {
              input message: 'Approve to apply the Terraform plan?'
            }
            sh 'terraform -chdir=terraform-infra apply -auto-approve'
          } else if (params.action == 'destroy') {
            if (!params.autoApprove) {
              input message: 'Approve to destroy the infrastructure?'
            }
            sh 'terraform -chdir=terraform-infra destroy -auto-approve'
          } else {
            error("Invalid action selected: ${params.action}")
          }
        }
      }
    }
  }
}

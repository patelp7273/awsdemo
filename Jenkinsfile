pipeline {
    agent {
        node {
            label 'master'
        }
    }
    environment {
        AWS_DEFAULT_REGION="us-east-1"
        TESTAWS=credentials('awscred') 
    }
    tools {
        terraform 'terraform'
    }
    stages {
        stage('checkout repo') {
            steps{
                git credentialsId: 'GitHub-ID', branch: 'main',url: 'https://github.com/patelp7273/awsdemo'
            }
        }
        stage('init') {
            steps {
                sh  """
                    terraform init -backend=true -input=false
                    """
            }
        }
        stage('validate') {
            steps {
                sh  """
                    terraform validate
                    """
            }
        }
        stage('plan') {
            steps {
                sh  """
                    terraform plan -out=tfplan -input=false 
                    """
                script {
                  timeout(time: 10, unit: 'MINUTES') {
                    input(id: "Deploy Gate", message: "Deploy ${params.project_name}?", ok: 'Deploy')
                  }
                }
            }
        }
        stage('apply') {
            steps {
                sh  """
                    terraform apply -lock=false -input=false tfplan
                    """
            }
        }
    }
}

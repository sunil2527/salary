@Library('test') _
pipeline {
    agent any
    tools {
  terraform 'terraform'
}
    stages {
        stage('checkout') {
            steps {
              script{
              checkout.repo_clone("Repo_Url": "https://github.com/Snatak-Fantastic4/IAC-Terraform.git","Repo_Branch": "dev", "Repo_Credential": "newtoken")
              }
            
            }
        }
        stage('terraform_format_check') {
            steps{
                 dir('environment/Dev_Infra'){
                 script {
               terraform.tf_command("command": "fmt")
            }
        }
        }
        }
        stage('terraform Init') {
            steps{
                 dir('environment/Dev_Infra'){
                 script {
                terraform.tf_command("command": "init")
            }
        }
        }
        }
        stage('Parallel') {
            parallel {
              stage('credScanner') {
                steps {
                 script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                  credScanner.cred("repourl": "https://github.com/Snatak-Fantastic4/IAC-Terraform.git","branchName": "dev","token": "${token}")
                    }
                }
            }
        }
          stage('linting') {
            steps{
                dir('environment/Dev_Infra'){
                     script {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                        linter()
                }
                }
            }
        }     
            }
        }
        }
        
        stage('terraform validate') {
            steps {
                 dir('environment/Dev_Infra'){
                 script {
                terraform.tf_command("command": "validate")
                 }
            }
        }
        }
        stage('terraform plan') {
            steps {
                 dir('environment/Dev_Infra'){
                 script {
                terraform.tf_command("command": "plan")
            }
            }
        }
        }
          stage('terraform apply') {
            steps {
                 dir('environment/Dev_Infra'){
                 script {
                terraform.tf_command("command": "apply -auto-approve")
            }
            }
        }
        }
        
        }
    }





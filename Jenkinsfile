pipeline {
    agent any

    stages {
        stage("Prep") {
            steps {
                git(
                    url: "https://github.com/ramywageh/test-project.git",
                    branch: "main",
                    
                )
            }
        }
        stage("Terraform init") {
            steps {
                dir("${TERRAFORM_DIR}") {
                    sh 'terraform init'
                }    
            }
        }
    }
}            
pipeline {
    agent any

    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
        choice(name: 'action', choices: ['apply', 'destroy'], description: 'Select the action to perform')
    }

    environment {
        TERRAFORM_DIR = "terraform/"
        ANSIBLE_PLAYBOOK = "ansible/playbook.yml" // Note: Unused in the script
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ramywageh/test-project.git'
            }
        }
        stage('Terraform Init') {
            steps {
                dir("${TERRAFORM_DIR}") {
                    sh 'terraform init'
                }
            }
        }
        /*stage('Plan') {
            steps {
                dir("${TERRAFORM_DIR}") {
                    script {
                        try {
                            sh 'ls -la' // Debug: Check directory contents
                            sh 'terraform plan -out tfplan'
                            sh 'terraform show -no-color tfplan > tfplan.txt'
                        } catch (Exception e) {
                            error "Terraform plan failed: ${e.message}. Ensure .tf files exist in ${TERRAFORM_DIR}."
                        }
                    }    
                }
            }
        }*/
        stage('Apply / Destroy') {
            steps {
                dir("${TERRAFORM_DIR}") {
                    script {
                        if (params.action == 'apply') {
                            if (!params.autoApprove) {
                                def plan = readFile "${TERRAFORM_DIR}/tfplan.txt"
                                input message: "Do you want to apply the plan?",
                                      parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
                            }
                            sh 'terraform apply tfplan'
                        } else if (params.action == 'destroy') {
                            sh 'terraform destroy --auto-approve'
                        } else {
                            error "Invalid action selected. Please choose either 'apply' or 'destroy'."
                        }
                    }
                }
            }
        }
    }
}
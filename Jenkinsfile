pipeline {
    agent any

    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
        choice(name: 'action', choices: ['apply', 'destroy'], description: 'Select the action to perform')
    }

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION    = 'ap-south-1'
        TERRAFORM_VERSION = "1.9.2"
    }
    
    options {
        // Increase timeout for the whole pipeline or stages if needed
        timeout(time: 20, unit: 'MINUTES')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ramywageh/test-project.git'
            }
        }
        stage('Install Terraform') {
            steps {
                script {
                    // This block waits for the shell script to fully complete
                    def result = sh(
                        script: '''
                            set -e
                           
                            echo "Downloading Terraform..."
                            curl -s -O https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip
                           
                            echo "Unzipping..."
                            unzip -o terraform_${TERRAFORM_VERSION}_linux_amd64.zip
                           
                            echo "Moving binary to /usr/local/bin..."
                            sudo mv terraform /usr/local/bin/
                            
                            echo "Verifying installation..."
                            terraform version
                            
                            echo "Cleaning up..."
                            cd /
                            rm -rf /tmp/terraform-install
                        ''',
                        returnStatus: true
                    )

                    if (result != 0) {
                        error("Terraform installation failed.")
                    }
                }
            }
        }
        stage('Terraform init') {
            steps {    
                sh 'terraform init'
            }
        }
        stage('Plan') {
            steps {
                sh 'terraform plan -out tfplan'
                sh 'terraform show -no-color tfplan > tfplan.txt'
            }
        }
        stage('Apply / Destroy') {
            steps {
                script {
                    if (params.action == 'apply') {
                        if (!params.autoApprove) {
                            def plan = readFile 'tfplan.txt'
                            input message: "Do you want to apply the plan?",
                            parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
                        }

                        sh 'terraform ${action} -input=false tfplan'
                    } else if (params.action == 'destroy') {
                        sh 'terraform ${action} --auto-approve'
                    } else {
                        error "Invalid action selected. Please choose either 'apply' or 'destroy'."
                    }
                }
            }
        }

    }
}